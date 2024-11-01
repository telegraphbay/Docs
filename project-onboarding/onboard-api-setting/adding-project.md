# Adding Project

Here are some of the interfaces and SQL involved in the onboard project.

## 1. S3 web2 trigger about user\_event type

* desc: Trigger s3 web2 production user\_event Standard Table Data
* method: post
* params:&#x20;
  * projectName: the name of project
  * businessType: Default user\_event
  * tables: Default \["user\_event", "asset\_flow", "session\_event", "session\_stats"]
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/fga/connector-config/connection/web2/standard/etl' \
--header 'Content-Type: application/json' \
--data '{
   "projectName": "Arc8",
   "businessType": "user_event",
   "tables": ["user_event", "asset_flow", "session_event", "session_stats"]
}
'
```



## 2. S3 web2 trigger about account\_mapping type

* desc: Trigger s3 web2 production account\_mapping Standard Table Data
* method: post
* params:&#x20;
*
  * projectName: the name of project
  * businessType: Default account\_mapping
  * tables: Default \["user\_wallet", "user"]
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/fga/connector-config/connection/web2/standard/etl' \
--header 'Content-Type: application/json' \
--data '{
   "projectName": "Arc8",
   "businessType": "account_mapping",
   "tables": ["user_wallet", "user"]
}
'
```

## 3. Create project

* desc: Create a new project.
* method: post
* params:&#x20;
*
  * name: the name of project
  * userId：For those implementing the API, a default of 186 is sufficient
  * ecosystemId: Default 415
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project' \
--header 'Content-Type: application/json' \
--data '{
   "names":["Gamee"],
   "userId":186,
   "ecosystemId":415
}'
```

## 4. Update Project

* desc: Update project information, usually used to hide a project or modify a project as a subproject.
* method: post
* params:&#x20;
*
  * projectId: the id of project
  * update：Contains information about the new project field. enum: active,parent\_id
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/update' \
--header 'Content-Type: application/json' \
--data '{
   "projectId": 435,
   "update": {
       "parent_id": 111
   }
}'
```



## 5. Create project mapping(mysql)

* desc: Create a record in mysql's project mapping, mainly for the mapping information needed on the TB application.
* method: post
* params:&#x20;
  * userId: For those implementing the API, a default of 186 is sufficient
  * projectId：the id of project
  * protocolName: the name of protocol in footprint (web3)
  * protocolSlug: the slug of protocol in footprint (web3)
  * website: the website of protocol
  * projectCategory: the category of project. enum: GameFi/NFT
  * icon: the icon of protocol
  * contracts: the contracts of protocol
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/protocol/submit' \
--header 'Content-Type: application/json' \
--data '{
   "userId":186,
   "projectId":435,
   "protocolName":"Mocaverse",
   "protocolSlug": "mocaverse",
   "website":"https://www.mocaverse.xyz/",
   "projectCategory":"GameFi",
   "icon": "https://static.footprint.network/ab/Mocaverse.png",
   "contracts":[{"chain": "Ethereum", "contractAddress": "0x85d0a718621906ffd85f88701283230cef22894e"}, {"chain": "Ethereum", "contractAddress": "0x59325733eb952a92e069c87f0a6168b29e80627f"}]
}'
```

## 6. Create project mapping(iceberg)

desc: The following SQL needs to be run on trino to create a project\_mapping record

```sql
insert into iceberg.prod_silver.project_mapping  
select 'gamee' as protocol_slug, 'Gamee TG' as project ,'animoca' as ecosystem
```

## 7. Refresh project cache

* desc: Update the cache of project information, the cache is kept for 1 day, which involves trino query. This is to increase the speed of interface queries and reduce the pressure on trino.
* method: post
* params:&#x20;
  * redisKeys: the rediskey of needing to refresh. The values need to be populated in this format: \["project\_detail\_{protocol\_slug}"]. For example \["project\_detail\_gamee"]，the protocol\_slug is gamee/mocaverse/..
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/custom/data/del/key' \
--header 'Content-Type: application/json' \
--data '{
   "redisKeys": ["project_detail_{protocol_slug}"]
}'
```

## 8.Refresh twitter ETL

* desc: Refreshing s3 twitter standard table production

```sql
code in dags/src/domain/s3_connector/twitter_etl.py  
method: TwitterETL(project, 'twitter_tweets_daily_stats').process()
code in dags/src/domain/s3_connector/twitter_etl.py  
method: TwitterETL(project, 'twitter_daily_stats_blaze').process()

```

## 9. Refresh discord ETL

* desc: Refreshing s3 discord standard table production

```sql
code in src/domain/s3_connector/discord_etl.py 
method: DiscordETL(project, 'discord_channel_daily_stats').process()
code in src/domain/s3_connector/discord_etl.py 
method: DiscordETL(project, 'discord_daily_stats').process()
```

## 10. Assign User Role

* desc: Configure the user's role, which is currently project/general/ecosystem.
* method: post
* params:&#x20;
*
  * metabaseId: the id of user&#x20;
  * projectIds：the projectId list of user, if user is a project role
  * role: project/general/ecosystem
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/user/access/assign' \
--header 'Content-Type: application/json' \
--data '{
   "metabaseId": 24823,
   "projectIds": [153],
   "role": "general"
}'
```

\
ps: The API will change the logic when the team feature goes live.
