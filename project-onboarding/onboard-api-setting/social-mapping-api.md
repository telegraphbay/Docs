# Social Mapping API

1. ### API Description：

This API is used to configure the binding of projects with Discord and Twitter accounts **which have already been uploaded to S3**.

&#x20;The process for this interface is as follows (The processing time for the API will be within 2 minutes and will depend on the number of linked accounts.):

1. Bind Social mapping information
2. Create Social raw table
3. Refresh the raw table view



1. ### Using the API:
   1. URL \[POST] :&#x20;
      1. prod:[https://www.telegraphbay.app/api/v1/project/social/mapping](https://www.telegraphbay.app/api/v1/project/social/mapping)
      2. staging: [https://www.staging.telegraphbay.app/api/v1/project/social/mapping](https://www.staging.telegraphbay.app/api/v1/project/social/mapping)
   2. Headers:
      1. x-token: can contact Duke or Alex to obtain the token&#x20;
   3.  Request body \[Json]:

       1. projectId: project id, You can obtain this information through Trino queries:

       `select * from mysql.metabase.project`&#x20;

       1. socialMapping:\
          – discord: discord service id list, eg: \["Mocaverse"]\
          – twitter: twitter account base info list , eg: \[{"handler": "Moca\_Network", "handle\_id": "1586321159745720321"}]\
          – – handler: handler name, this field will serve as the table name for creating a new raw table, eg: raw\_data.animoca\_{projectName}.{lower(handler)}\_twitter\_tweet\_metrics.\
          – – handle\_id: twitter handler id, ensure that the handler ID matches the one uploaded by Blaze, eg: s3://web2-raw-data-prod/social/twitter/{handle\_id}/tweet\_with\_metrics

example:

```json
curl --location 'https://www.telegraphbay.app/api/v1/project/social/mapping' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--header 'Cookie: metabase.DEVICE=***' \
--data '{  
  "projectId": "153",
  "socialMapping": {
      "discord": ["Mocaverse"],
      "twitter": [
          {"handler": "Moca_Network", "handle_id": "1586321159745720321"}
      ]
   }
}
'

```



3. ### verify if the configuration was successful
   1. check if the Social Mapping for your project has been configured successfully

```sql
select id, name, social_mapping  from mysql.metabase.project
```

2. verify if the raw data for the newly configured Discord and Twitter accounts can be accessed.

<pre class="language-sql"><code class="lang-sql">-- discord__raw__messages
select * from iceberg.animoca.discord__raw__messages 
where project in (
<strong>    select distinct name  from mysql.metabase.project 
</strong><strong>    where id={projectId}
</strong>)

 -- twitter__raw__user_detail
 
select * from iceberg.animoca.discord__raw__messages where project in (
    select distinct  name  from mysql.metabase.project where id={projectId}
)

select * from iceberg.animoca.twitter__raw__user_detail  where project in (
    select distinct  name  from mysql.metabase.project where id={projectId}
)
</code></pre>

4. ### Additional information:
   1. After the next round of task production, the Discord and Twitter standard tables will generate updated configurations for social data.
   2. There is no need to manually add the Portco project. After uploading the raw data, it will automatically display in the raw data table.

\
