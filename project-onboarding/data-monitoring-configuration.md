# Data Monitoring Configuration

1. Purpose of Data Monitoring

The role of data monitoring is to track the change rate of TB dashboard metrics to ensure they stay within reasonable ranges. Any metrics that fall outside these ranges are flagged for attention and notification, prompting further investigation.

Calculation Logic for Rate of Change：&#x20;

$$
objectValue.crrurentResult - previousObjectValue.crrurentResult previousObjectValue.crrurentResult
$$

2.  ### Four Steps to Use Data Monitoring

    1. Setting Up Data Monitoring Rules
    2. Triggering validation
    3. Observation results
    4. Additional script supplement


3. **Step 1 - Setting Up Data Monitoring Rules**

You can choose to manage data monitoring rules using either the API or by connecting to the MySQL database.

1. Using API
   1. Method: post
   2. Parameters:
      1. project: The project being validated
      2. dashboardId: The dashboardId to be validated
      3. chartId: The chartId to be validated
      4. parameters: Parameters passed to the chart
      5. minThresholds: Lower limit threshold for change; if the change exceeds this threshold, it is considered abnormal, no validation when the value is less than 0
      6. maxThresholds: Upper limit threshold for change; if the change exceeds this threshold, it is considered abnormal
      7. status: The status of this rule, either 'enable' or 'disable'; if disabled, the rule will not be validated
      8. allowNullable: Indicates if this metric is allowed to be null
      9. needWarn: Whether a yellow warning should be displayed on the TB if this metric is abnormal
      10. label: Indicates if the metric is frequently changing; 'active' or 'inactive'. If set to 'active' and the metric has not changed for over 3 days, it is considered abnorma

Code:

```json
--header 'Content-Type: application/json' \
--data '{
"project": "Phantom Galaxies",
"dashboardId": 8471,
"chartId": 43913,
"parameters": {
"project": "Phantom Galaxies"
},
"minThresholds": -1,
"maxThresholds": 0.1,
"status": "enable",
"allowNullable": true,
"needWarn": 1,
"label": "active"
}'
```

#### 2. Managing through MySQL database &#x20;

You can reach out to Bland for the connection setup details.After successfully connecting via MySQL, you can directly add or update data monitoring rules on the data.



_Note: The data\_monitoring\_metric\_rule needs to maintain rules with status='enable' (enabled validation rules), ensuring uniqueness based on project, dashboard\_id, chart\_id, and parameters\_hash._



Table Path：

* rules：metabase.data\_monitoring\_metric\_rule
  * id: Rule id
  * project: The project being validated
  * dashboard\_id:  The dashboardId to be validated
  * chart\_id:  The chartId to be validated
  * parameters: Parameters passed to the chart
  * parameters\_hash: Parameters MD5 encoded and converted to uppercase —- MD5(parameters).toUpperCase()
  * min\_thresholds: Lower limit threshold for change; if the change exceeds this threshold, it is considered abnormal, no validation when the value is less than 0
  * max\_thresholds: Upper limit threshold for change; if the change exceeds this threshold, it is considered abnormal
  * status: The status of this rule, either 'enable' or 'disable'; if disabled, the rule will not be validated
  * allow\_nullable: 0/1, Indicates if this metric is allowed to be null
  * need\_warn: 0/1, Whether a yellow warning should be displayed on the TB if this metric is abnormal
  * label: Indicates if the metric is frequently changing; 'active' or 'inactive'. If set to 'active' and the metric has not changed for over 3 days, it is considered abnormal
* record：metabase.data\_monitoring\_metric\_record
  * id: Record id
  * record\_time: Execution Time for Validation
  * project: The project being validated
  * dashboard\_id:  The dashboardId to be validated
  * chart\_id: The chartId to be validated
  * title: Chart name
  * values:The result obtained from the chart, this field is deprecated, upgraded to object\_value
  * parameters: Parameters passed to the chart
  * parameters\_hash: Parameters MD5 encoded and converted to uppercase —- MD5(parameters).toUpperCase()
  * status: The validation result of this rule: "init" represents the initial check record, which needs to be compared with the results of the second round. "normal" indicates a normal change value, while "abnormal" signifies a change value that exceeds the range and is considered abnormal.
  * error\_type: Error type
  * error\_message: Error detail
  * actual\_change\_percent: Validation Results for this Rule
  * previous\_values: Previous validation results, This field has been deprecated and upgraded to previous\_object\_value
  * object\_values: The result obtained from the chart
  * previous\_object\_value: Previous validation results\


Querying all dashboards and charts currently displayed on TB:

```sql
with mainproject as (
   select distinct parent_id as project_id from project where parent_id  is not null
), menudata as (
  select sppc.project_id, spmc.menu_id, sppc.default_values, sppc.`values`, spm.title as menu_title, spf.key as filter_key, regexp_substr(spp.link, '[^/]+$') as uuid
   from
       standard_panel_page_config sppc
   left join
       standard_panel_page spp
   on sppc.page_id=spp.id
   left join
       standard_panel_menu_config spmc
   on sppc.menu_config_id=spmc.id
   left join
       standard_panel_menu spm
   on spmc.menu_id=spm.id
   left join
       standard_panel_filter spf
   on sppc.filter_id=spf.id
   where
       spp.type = 'dashboard'
   and (
     (sppc.project_id not in (select project_id from mainproject)) or
     (sppc.project_id in (select project_id from mainproject) and spm.value not in ('monetization', 'user-acquisition', 'user-engagement', 'user-retention'))
   )
), includedashboard as (
  select menudata.*, rd.id as dashboard_id, rd.name as dashboard_name from menudata
  left join
      report_dashboard rd
  on rd.public_uuid = menudata.uuid
), includechard as (
  select includedashboard.project_id, includedashboard.menu_id, includedashboard.dashboard_id, includedashboard.dashboard_name, includedashboard.menu_title, rdc.card_id, rc.name as card_name, rc.display as card_display, includedashboard.default_values, includedashboard.filter_key
   from
       includedashboard
   left join
       report_dashboardcard rdc
  on rdc.dashboard_id = includedashboard.dashboard_id
   left join
       report_card rc
   on rc.id=rdc.card_id
  where rdc.card_id is not null
), result as (
   select
       project_id,
       menu_id,
       menu_title,
       dashboard_id,
       dashboard_name,
       card_id,
       card_name,
       card_display,
       json_objectagg(filter_key, default_values) as parameter
   from
       includechard
   where
       filter_key is not null
   group by
       dashboard_id,
       dashboard_name,
       card_id,
       card_name,
       menu_id,
       menu_title,
       project_id
   order by
       project_id,
       menu_title,
       dashboard_id,
       card_id
), result_with_project as (
   select
       p.name as project,r.*
   from
       result r
   left join
       project p
   on r.project_id = p.id
)
select * from result_with_project
```



Query the threshold config of the charts:

```sql
select * from metabase.data_monitoring_metric_rule
```

#### Query the records of the verify:

```sql
select * from metabase.data_monitoring_metric_record
```



Insert new rule example：

\
Please make sure to check for the existence of unique keys before inserting. If they exist, consider using an update instead. The unique keys to check for are: project, dashboard\_id, chart\_id, and parameters\_hash.



```sql
insert into   metabase.data_monitoring_metric_rule 
(project, dashboard_id, chart_id, parameters, parameters_hash, min_thresholds, max_thresholds,status,updated_at,created_at,allow_nullable,need_warn,label)
values('ProjectG',8503,44086,'{"collection_contract_address": ["0x59325733eb952a92e069c87f0a6168b29e80627f"]}','0553A8764BB766DC35C08C5AB2A726AA',0.00001,0.2,'disable',current_timestamp,current_timestamp,0,1,'active')
```



updated rule example：

```sql
update  metabase.data_monitoring_metric_rule set max_thresholds=0.0005, status='enable' where 
project = 'ProjectG' 
  and dashboard_id=8503 
  and chart_id = 44086 
    and parameters_hash='0553A8764BB766DC35C08C5AB2A726AA'

```



4. **Step 2 - Triggering validation**

The data monitoring validation will run once daily at 18:00 UTC+8:00, taking approximately one hour to complete the validation and send email notifications.\


_Task scheduling is managed through Airflow, and the code is located in this directory：https://github.com/telegraphbay/animoca-airflow-dbt/blob/main/dags/src/domain/data\_monitroing/dag.py_&#x20;

_code: schedule\_interval='0 10 \* \* \*'_

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcP48_HBSutWFFO50C04Z3k8CUsxnVQQQebt1d6EugwMGkDO1zsIE3kKr1Ja-Y7QJnjrgOvbQTeC5El2CSnyLkWHrVNxhYYJL7CBrDu8Lz72MmJ5cRX8gGTZECBYhxpJxqKypWJzAm8kHODZ3bhaMEmjT8p?key=L7ZdpafmTJWlMK5TBN88LQ" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfs1zTFaMXO_wazQ6j3T7j8brkMafFNBxmiX_5rv7nNwHuyRae1y4N76r7Mq_AcEvakX1Y9AOurgoqILQyPSsouklW1xjOpPVJgAZ51xiv1HgVjiTTMBNFMcS3eTCOwFMZ1m0inz7IKyl3RfThFE5R9nM98?key=L7ZdpafmTJWlMK5TBN88LQ" alt=""><figcaption></figcaption></figure>

In addition to scheduled checks, we also support on-demand triggers. By logging into [Airflow](https://airflow.staging.telegraphbay.app/tree?dag_id=data_monitoring_alert) and clicking the trigger button, a new round of data monitoring validation will begin.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeZVux8w5laYZTLTfpS4LPsVI3PUve4-JlTzvYyTQ7daxsxcIhfiKbeKH3wJvKhkXETepO7-ef7qZ2GA05DrFT4E94FcJpHUW-sSSaaN296e-EJuIeG39h431_esRwFntmc_hAqUZWYUNLS3PEYePw8n1lf?key=L7ZdpafmTJWlMK5TBN88LQ" alt=""><figcaption></figcaption></figure>

5. Step 3: Observation results

In addition to checking the validation results via email, you can also view more details on the [dashboard\
](https://www.staging.telegraphbay.app/ab/dashboard/@Bond/QC-Job-Status-Dashboad?sync_time-82772=past1days~#type=dashboard)

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeR6eyUvbK2ioY-ZxqQdqMaA0sy8u8UEsJQR-c0OlTafPwK9VHoPVxYD_p7HFFILwV355UTmlZyvxMp7UpaBaamzxo7TvZwcvC0-cSjZueZTSH2TJ_DTC6FCnG54sCLc0SaXYR8_pDfHvjhbnKlZPSn9tM?key=L7ZdpafmTJWlMK5TBN88LQ" alt=""><figcaption></figcaption></figure>

4. **Step 4: Additional script supplement**

* Threshold calculation script

logic: Utilizing historical values of the metric to compute the average and standard deviation, then determining the upper limit based on the 3-sigma rule\


```python
def sigma_cal(values):
    percents = []
    for index in range(len(values) - 1):
        if values[index] != 0:
            percents.append((values[index + 1] - values[index]) / values[index] * 100)
        else:
            percents.append(0)
    mean = np.mean(percents)


    # cal standard deviation
    std_dev = np.std(percents)


    # cal the threshold according to the 3-sigma rule
    lower_bound = mean - 3 * std_dev
    upper_bound = mean + 3 * std_dev


    threshold = max(abs(lower_bound), abs(upper_bound))
    print(f'average: {mean}')
    print(f'Standard Deviation: {std_dev}')
    print(f'3-sigma Lower bound (warning threshold): {lower_bound}')
    print(f'3-sigma upper_bound (warning threshold): {upper_bound}')
    return upper_bound - lower_bound
```





\


\
\


\


\
