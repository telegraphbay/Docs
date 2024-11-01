# How to Config Standard Panel



## 1. Class Design

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXd3qKFN0KMZOUIRbDSbn5cizBwYczsC5AlQXlyXYzXylVkgdzY10unZKADD-DRdc454K-5B99n75tcHXxJdaGNBmoadEvOvep0OECvbQ7_3sGKUrLXag8rgdMgPcEOZ7xOpgmwexbkytvF_uWpqpTafsNuz?key=f68GU7O9zKUL2o8VLGBdVw" alt=""><figcaption></figcaption></figure>

* menu: store menu entity
* page: store page entity (dashboard or native page)
* filter: store dashboard filter param entity ( it can config multiple mode in select)
* menu\_config: It relates menu items to projects and defines their hierarchical structure and order
* page\_config: linking pages to menu configurations and specifying associated filters. (config default collection and selectable collection list per dashboard)

## 2. Interface list

### 1. get standard panel data

name: get standard panel data

method: get

desc: query the menu-page mapping configuration for each project in TB

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/pageConfig/getPageData?projectId=153'
```



### 2. get project list

name: get project list

method: get

desc: query the project list in TB

column: The ecosystemId is fixed at 415.

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/project/list?ecosystemId=415'
```

ps: The system has only one ecosystem, just pass the reference 415

### 3. get menu

name: get menu

method: get

desc: query all menu entity from db

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/menu/get'
```

### 4. add menu

name: add menu

method: post

desc: add a menu entity to db

column：

* type: Currently, “project” is used, with support for “ecosystem” menus to be added in the future.

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/menu/add' \
--header 'Content-Type: application/json' \
--data '{
   "type": "project",
   "title": "NFT Primary Sales",
   "value": "nft_primary_sales",
   "icon": "",
   "desc": ""
}'
```

### 5. update menu

name: update menu

method: post

desc: update a menu entity to db

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/menu/update' \
--header 'Content-Type: application/json' \
--data '{
   "id": 1,
   "data": {
       "archived": false
   }
}'
```

### 6.get menu config

method: get

desc: query all menu\_config entity from db

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/menuConfig/get'
```



### 7.add menu config

method: post

desc: add a menu\_config entity to db

column:&#x20;

* level: 1,2,3 represents the first, second and third level menus. eg. assets is 1, NFT is 2, NFT scales is 3
* order: 1 is at the top of the list
* parent\_id: For level 1, fill in with null. For level 2 and 3， fill in with the menuId of the parent menu.

code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/standardPanel/menuConfig/add' \
--header 'Content-Type: application/json' \
--data '{
   "project_id": 428,
   "menu_id": 26,
   "parent_id": 1,
   "order": 2,
   "level": 2
}'
```

### 8. update menu config

name: update menu config

method: post

desc: update a menu\_config entity to db

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/menuConfig/update' \
--header 'Content-Type: application/json' \
--data '{
   "id": 1,
   "data": {
       "archived": false
   }
```

### &#x20;9. get page

method: get

desc: query all page entity from db

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/page/get'
```



### 10. add page

method: post

desc: add a page entity to db

column：

* type: dashboard/native/page
* title: It serves merely as an identifier to recognize the page and has no practical use.
* link: In the dashboard's "Share" dialog, copy the simple link as shown in the image below.

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/page/add' \
--header 'Content-Type: application/json' \
--data '{
   "type": "dashboard",
   "title": "NFT Primary Sales - Motorverse",
   "link": "/public/dashboard/baefb88b-6234-4086-807b-ae57d3aa4611"
}'
```

Get dashboard link as the picture shown below:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfzT7w_t36NsnaVLvyEN5y3QEJTn7wQ4BHBTBjOleyMpZ4R13zjun6_vtKAKZv1cyTT9FyFWGRl8-Pmqmu_ejN9vwm7g6de5c59j8LySVqgVz982WZx85PLG_6NsdcdMuCR_CdnU52S5zhF5O6rRiRXXm8?key=f68GU7O9zKUL2o8VLGBdVw" alt=""><figcaption></figcaption></figure>

### &#x20;11. update page

method: post

desc: update a page entity to db

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/page/update' \
--header 'Content-Type: application/json' \
--data '{
   "id": 1,
   "data": {
       "archived": false
   }
}'
```

### 12.  get filter

method: get

desc: query all filter entity from db

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/filter/get'
```

### 13. add filter

method: post

desc: add a filter entity to db

column：

* key：The filter keys can be found in the dashboard URL, for example in this URL, the keys for filters include collection\_contract\_address and hours\_interval

eg.https://www.staging.telegraphbay.app/@Bond/NFT-Listing?collection\_contract\_address=0x59325733eb952a92e069c87f0a6168b29e80627f\&hours\_interval-80763=24

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/filter/add' \
--header 'Content-Type: application/json' \
--data '{
   "key": "collection_group",
   "type": "select",
   "mode": "single"
}'
```

ps: filter key will be found in dashboard url，&#x20;

https://www.telegraphbay.app/ab/dashboard/@Bond/User-Retention-Motorverse2?project=Motorverse\&cohort\_event=sign\_up\&retention\_event=login#type=dashboard



### 14. update filter

name:&#x20;

method: post

desc: update a filter entity to db

code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/standardPanel/filter/update' \
--header 'Content-Type: application/json' \
--data '{
   "id": 1,
   "data": {
       "type": "select"
   }
}'
```

### 15. get page config

method: get

desc: query all page\_config entity from db

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/pageConfig/get'
```

### 16. add page config

method: post

desc: add a page\_config entity to db

code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/standardPanel/pageConfig/add' \
--header 'Content-Type: application/json' \
--data '{
   "project_id": 153,
   "menu_config_id": 3,
   "page_id": 1,
   "filter_id": 4,
   "default_values": ["aaa"],
   "values": ["bbb", "bbbd"]
}
```

ps:

* To associate a page with a menu, you only need to provide the project\_id, menu\_config\_id, and page\_id; leave the other fields null.&#x20;
* If you need to configure a list for a filter within the dashboard, you must specify the project\_id, menu\_config\_id, page\_id, filter\_id, and values.&#x20;
* If you need to set default values for a dashboard filter, you should configure the project\_id, menu\_config\_id, page\_id, filter\_id, and default\_values.

### 17.update page conf

method: post

desc: update a page\_config entity to db

code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/standardPanel/pageConfig/update' \
--header 'Content-Type: application/json' \
--data '{
   "id": 1,
   "data": {
       "archived": false
   }
}'
```

### 18. copy menu config

method: post

desc: You can copy the data from the menu config to the new project id.

code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/standardPanel/menuConfig/copy' \
--header 'Content-Type: application/json' \
--data '{
   "source_project_id": 153,
   "target_project_id": 435
}'
```



### 19. copy page config

method: post

desc: You can copy the data from the page config to the new project id.

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/standardPanel/pageConfig/copy' \
--header 'Content-Type: application/json' \
--data '{
   "source_project_id": 153,
   "target_project_id": 435
}'
```



## 3. Best practice

### 1. Configure Menu Config

1. Invoke the get menu API to check what menus are available in the system. If none exist, use the add menu API to create a menu and obtain a menuId.
2. Use the get project list API to retrieve the projectId.
3. Configure the menu config by calling add menu config API or update menu config API. This sets up the association between the project and the menu.

\


### 2. Configure Page Config

1. Use the get menu config API to obtain the already configured menu config id.
2. Invoke the get page API to find out what pages are available in the system. If none exist, use the add page API to create a page and obtain a pageId.
3. Use add page config API or update page config API to configure the page config. This establishes the relationship between the project, menu, and page.



### 3. Configure Default Collection or Collection List in NFT Dashboard

1. Call the get filter API to see what filters the system has. If none exist, use the add filter API to create a filter and obtain a filterId. You can also set the filter's mode to multiple.
2. Use update page config API to configure the page config. This can associate the filterId, default values, and values (in the context of the NFT dashboard collection, the values in values are only supported if they match the current project's NFT collection).\
