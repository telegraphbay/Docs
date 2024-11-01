# Add Sub project



For the Gamee case, you need to add Gamee main project and turn Arc8 into subproject and add Game TG project. Follow the steps below.

1. Please use the [API](https://docs.google.com/document/d/1SV7ToMeMA35iW3ZjuzZYtPGgfXZ4qSk37FbBMw800vs/edit#bookmark=id.8dj2vievuh7t) to add the Gamee project.c
2. Update the Arc8 project to the main Gamee project.

params:&#x20;

parent\_id: The project id of the main project

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/update' \
--header 'Content-Type: application/json' \
--data '{
    "projectId": 423,
    "update": {
        "parentId": {the project id of Gameee}
    }
}
```

3. Please use the [API](https://docs.google.com/document/d/1SV7ToMeMA35iW3ZjuzZYtPGgfXZ4qSk37FbBMw800vs/edit#bookmark=id.8dj2vievuh7t) to add the Gamee TG sub project.c

The parameters to be passed are as follows:

```sql
{
  "names":["Gamee"],
  "userId":186,
  "ecosystemId":415,
  "parentId": {the project id of Gameee}
}
```

4. Please use the API to append Gamee's mapping record to the project\_mapping in MySQL (main project)
   1. sub project has web3 data mapping (e.g. nft)
      1. no changes to main project mapping record
      2. no sub project mapping record has to be added
   2. sub project doesn’t have web3 data mapping (e.g. no nft)
      1. no changes to main project mapping record
      2. no sub project mapping record has to be added

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/protocol/submit' \
--header 'Content-Type: application/json' \
--data '{
   "userId":186,
   "projectId":{the project id of gamee project},
   "protocolName":"Gamee",
   "protocolSlug": "gamee",
   "website":"https://www.gamee.com",
   "projectCategory":"GameFi",
   "icon": "https://static.footprint.pesnetwork/ab/GAMEE.png",
   "contracts":[
       {"chain": "Polygon", "contractAddress": "0x3a85ac1a9344a5940c614b4b79fe74b40469f936"},
       {"chain": "Polygon", "contractAddress": "0xcf32822ff397ef82425153a9dcb726e5ff61dca7"},
       {"chain": "Ethereum", "contractAddress": "0x1fe1ffffef6b4dca417d321ccd37e081f604d1c7"},
       {"chain": "Polygon", "contractAddress": "0x5b30cc4def69ae2dfcddbc7ebafea82cedae0190"},
       {"chain": "Ethereum", "contractAddress": "0xd9016a907dc0ecfa3ca425ab20b6b785b42f2373"},
       {"chain": "BNB Chain", "contractAddress": "0x84e9a6f9d240fdd33801f7135908bfa16866939a"},
       {"chain": "Polygon", "contractAddress": "0x60adeeabe344d34eb0473350192d7efef7176610"}
   ]
}'
```

5. Use SQL to Add Gamee TG project mapping in iceberg (sub projects)
   1. sub project has web3 data mapping (e.g. nft)
      1. corresponding protocol\_slug in FP data, map to the sub project
   2. sub project doesn’t have web3 data mapping (e.g. no nft)
      1. use main project protocol\_slug, map to the sub project

```sql
insert into iceberg.prod_silver.project_mapping  
select 'gamee' as protocol_slug, 'Gamee TG' as project ,'animoca' as ecosystem
```



6. Please create a new menu and game configurations for Gamee (main project). The copy API can be utilized to replicate the existing configuration from the Arc8 project. Can only be called copy API once.
   1. [copy menu config](https://docs.google.com/document/d/1zXT\_0NWULUqU1qqj1SE7XSqNcbPmP7sMNtJq0G9jTFc/edit#bookmark=id.xztzx8wznque)
   2. [copy page config](https://docs.google.com/document/d/1zXT\_0NWULUqU1qqj1SE7XSqNcbPmP7sMNtJq0G9jTFc/edit#bookmark=id.g4jcxlj5eyfz)
7. Use [API](https://docs.google.com/document/d/1SV7ToMeMA35iW3ZjuzZYtPGgfXZ4qSk37FbBMw800vs/edit?pli=1#bookmark=id.sw5bgl5td65s) to refresh project cache and You can see the dashboard data in TB
8. Configure the connector in integration page on TB and trigger web2 data generation of Gamee TG Project (sub project)
9. Go to data-model to define ETL and check standard tables

**Clean up main project data/old data**

10. Update raw view (sub project)
11. Create the view of Game TG project
    1. Need to execute 1 functions (create\_standard\_view)  with a [script](https://docs.google.com/document/d/1uRT2zAAJT9VBM1iFtrSg0cntx6E77Pik\_owAnQ-FSAM/edit)&#x20;

\
