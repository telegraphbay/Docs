# Key Steps of Project On-boarding

&#x20;This applies to both adding projects and sub-projects.

## 1. create or update a project

* [add](adding-project.md#id-3.-create-project)
* [update](adding-project.md#id-4.-update-project)

## 2. create project mapping

* [mysql](adding-project.md#id-5.-create-project-mapping-mysql)
* [iceberg](adding-project.md#id-6.-create-project-mapping-iceberg)

## 3. Data

### - Web2 Data

* S3&#x20;
  * Defining the schema for a Hive table is determined by the schema of the actual data. ([link](../../platform-architecture/quick-start-guide-to-trino.md))
  * [Github](https://github.com/telegraphbay/data-model) ETL（just staging can auto trigger）
  * Configure the project for ETL mapping within the code.&#x20;
  * You need to run a script to trigger data generation, or wait until the next day  (UTC+5)  for automatic data generation.
    * [user\_event](adding-project.md#id-1.-s3-web2-trigger-about-user\_event-type)
    * [account\_mapping](adding-project.md#id-2.-s3-web2-trigger-about-account\_mapping-type)
* social(raw data in S3)
  * Generate the table&#x20;
  * Need to execute 2 functions (build\_discord\&build\_twitter)  with a [script ](../../platform-architecture/trino-script.md)
  * Configure the project for ETL mapping within the code. (alex)
  * You need to run a script to trigger data generation, or wait until the next day  (UTC+5)  for automatic data generation.
    * [refresh twitter ETL](adding-project.md#id-8.refresh-twitter-etl)
    * [refresh discord ETL](adding-project.md#id-9.-refresh-discord-etl)
* Airbyte&#x20;
  * Add menu ([dashboard config API](../../platform-architecture/how-to-config-standard-panel.md))
    * &#x20;integration page
  * Configure the connector on TB and trigger data generation
    * [BigQuery](../web-2-data-connectors/bigquery-setup-instructions.md)
    * [Snowflake](../web-2-data-connectors/snowflake-setup-instructions.md)
    * [PostgresSQL](../web-2-data-connectors/postgresql-setup-instructions.md)
    * [MySQL](../web-2-data-connectors/mysql-setup-instructions.md)
    * [S3](../web-2-data-connectors/s3-file-upload-instructions.md)
    * [GA 4](../web-2-data-connectors/google-analytics-4-setup-instructions.md)
  * [Github](http://github.com/telegraphbay/data-model) ETL（just staging can auto trigger）
  * You can perform quality control (QC) at this [link](../web-2-data-connectors/quality-control-procedure.md).&#x20;

### - Web3&#x20;

* Ready for data
* More collections data must be sync from footprint.

## 3. View in Iceberg

* Project View
  * standard&#x20;
    * Need to execute 1 functions (create\_stander\_view)  with a[ script ](../../platform-architecture/trino-script.md)
  * raw&#x20;
    * s3 web2 (manually）
      * l[ink](../../platform-architecture/quick-start-guide-to-trino.md)
    * social
      * Need to execute 2 functions (discord\_view\_update\&twitter\_view\_update)  with a[ script ](../../platform-architecture/trino-script.md)

## 4. Show in TB

* add menu ([dashboard config API](../../platform-architecture/how-to-config-standard-panel.md))
  * new&#x20;
    * [add menu config](../../platform-architecture/how-to-config-standard-panel.md#id-7.add-menu-config)
    * [add page config](../../platform-architecture/how-to-config-standard-panel.md#id-16.-add-page-config)
  * copy
    * [copy menu config](../../platform-architecture/how-to-config-standard-panel.md#id-18.-copy-menu-config)
    * [copy page config](../../platform-architecture/how-to-config-standard-panel.md#id-19.-copy-page-config)
* dashboard config
  * &#x20;config the dashboard permission in TB dashboard config menu
* Refresh project cache
  * [API](adding-project.md#id-7.-refresh-project-cache)

## 5. User Role Config

* [API](adding-project.md#id-10.-assign-user-role)
