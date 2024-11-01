# Team Features Setting

## I. SSO Design

Below is the design of the class diagram for SSO, which shows the relationship between the modules. You can [click](https://www.plantuml.com/plantuml/png/hLJ12jim3BtxAtHS1swwmowXbDxs6S59HElXsi6MjvNHVnyMZOiWD2diQ4kotjDxyZcHYPHlJf4bXosa5634P2zUAVHawGufzpzne2wYCsI3JrSxmLy5y1Nis8BFjnuZ78yNz1WDuNWwSN2kiwCuqdr2Q1h82MCJCOKS1-ICoKJqrBtgvAqiZy5XSuaXJ\_MH7-Ma0BHM08aCvTJ2BnE7gubVlXzsu0E1pywfHoKtrtQ6ADe-4swWLgiOJMtnxN3OoR0TmNggVJbO6xsWbJe1UmKrpuvnhY3bHnzp5Kzh2prExBovOPqVtnUshO1TmStuHqn32uH\_Q4q3W26YeH2xWiLwUYVF5jIpCyZq3XipsMQ\_ttEaC96mfI8fXS6ojnSKqrf05U4dOUQxMRmfE3lS\_zC6pIiGt7E7mcyiC5-7ERtEKkvBK\_tucGLBHzX0WXEbbwG4dplhdNNM5vztvwXrdXP\_DiK3VPV0p3V5drAhwc9nk\_3rQ7sgCYt4PRqwen\_Br\_qF) on this link to see the details\
\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXd9rRhte_Yw1xO4X0Hui7BFK27MhkuA4n9p0I-lqDklD6545s1yTFVcesRGzBx384SIZrvFvU2JAtfZBKgTIGAq2CVgyT07sfFnPWXCNybPOOECGXBXukDpXfTsHhIEF18sS9ujcKHH8KdeQENiGs_odzKS?key=kW6BpWd1FahNqy-g-zFTHg" alt=""><figcaption></figcaption></figure>

## II.   The Flow of Team Feature

Users can walk through the TEAM FEATURES in the following order. All steps can be realized through the API. But from the second to the fourth step can be realized in the TB fronted.

1. Create a project via API
2. Assign users to the project and users can see the project in TB
3. Assign the project Manager role to the user and users can manage project in TB
4. Assign the dashboard into the project and users can view the project dashboard in TB
5. Last, we can check the permission of users in TB via API&#x20;

## III. Lists of Interface&#x20;

There are 12 api's that can be called using postman by passing x-token. In the process of using the API, you will encounter some enumeration parameters, please use these parameter values to use the following APIs



* userId: the id of user
* projectId: the id of project （using get projects API)
* ProjectRole: Manager / Data User / Viewer
* ProjectAttribute: view / create / manage / delete / assign-user / remove-user / set-permission / add-dashboard / remove-dashboard
* x-token: jyrmRy2zH4EM5dfPH42m

### 1.  get projects

* desc: Query the project list in TB
* method: get
* param: The ecosystemId is fixed at 416 in the testing team feature.
* code:

```sql
curl --location 
'https://www.staging.telegraphbay.app/api/v1/project/list?ecosystemId=416'
```

### 2. create project

* desc: Create a new project, in order to test the TEAM FEATURE, specially made out of the new test with the.&#x20;
* method: post
* param:&#x20;
*
  * ecosystemId:416 (This is created to test the functionality of the team feature)
  * userId:186 (Constants passed by default)
* header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }
* code:&#x20;

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/create-new' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
    "names":["ProjectG"],
    "userId":186,
    "ecosystemId":416
    }'
```

### 3. assign user to project

desc: Add user to project with project permissions - default

method: post

params:&#x20;

projectRole: Manager, Data User, Viewer

header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }

code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/assign/user' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
    "projectId": 436, 
    "userAndRoles": [
        {
            "userId": 24828,
            "projectRole": "Manager" 
        }
    ]
}'
```

### 4. remove user from project

desc: Remove user from project and remove all project permissions

method: post

header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }

code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/remove/user' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
    "projectId": 436, 
    "userIds": [10]
}'
```

### 5. assign user project role

desc: Assigning project roles to users. Only the manager and viewer roles are allowed to be passed. only one role can exist for a user.

method: post

header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }

code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/assign/role' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
    "projectId": 436, 
    "userId": 10,
    "projectRole": "Manager"
}'
```

### 6. remove user project role

desc: Remove project roles from users

method: post

header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }

code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/remove/role' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
    "projectId": 436, 
    "userId": 10,
    "projectRole": "Manager"
}'
```

### 7. check user project action

desc: Check if a user has action permissions for an item

method: post

params:&#x20;

* attribute: optional. If not, return all the user's actions in the project. The project property, which is an enumerated value. It mainly refers to the action  that the project can do.
* projectId: optional. If not, return all the user's actions in TB

header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }

code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/user/attribute/permission' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
    "userId": 10,
    "projectId": 436,
    "attribute": "view"
}'
```

### 8. assign dashboard to project

* desc: We can add the dashboard to the project. If we join a project, we can get access to the project dashboard's view.
* method: post
* header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/assign/dashboard' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
   "projectId": 436,
   "dashboardIds": [8503]
}'
```

### 9. remove dashboard to project

* desc: We can remove the dashboard to the project. If we leave a project, we can not view the project dashboard.
* method: post
* header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/remove/dashboard' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
   "projectId": 436,
   "dashboardIds": [8503]
}'
```

### 10. query the users of project&#x20;

* desc: Returns the user information of a project, including id, email, name, role.
* method: post
* params:&#x20;
*
  * detail: false/true - If false, only the user id is returned.
* header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/users' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
   "projectId": 436,
   "detail": true
}'
```

### 11. query the dashboards of project&#x20;

* desc: Returns the dashboard information of a project, including id, name, uuid, create\_id, created\_at.
* method: post
* params:&#x20;
*
  * detail: false/true - If false, only the dashboard id is returned.
* header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/project/dashboards’ \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
   "projectId": 436,
   "detail": true
}'
```

### 12. query user Id in TB

* desc: Returns the user id & name in TB by searching
* ### method: post
* ### params:&#x20;
* text: search user email&#x20;
* header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/user/search’ \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
   "text": "animoca"
}'
```

### 13. add standard dashboard

* method: post
* params:
*
  * dashboardId: the id of dashboard
  * creatorId: the id of operator
* header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/dashboard/addStandardDashboard' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
   "dashboardId": 5039,
   "creatorId": 186
}'
```



### 14. query standard dashboards&#x20;

* method: post
* header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/dashboard/queryStandardDashboards' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data '{
}'
```



### 15. Add user to TB

* method: post
* params:
  * email: the email of user
  * name: (optional) the name which shows in TB. If empty, it will be filled the string before @ of email
  * password: (optional) the password of the user. If empty, it will be filled 20-digit random password obtained from uuid
  * header: { “x-token”: “jyrmRy2zH4EM5dfPH42m” }
* code:

```sql
curl --location 'https://www.staging.telegraphbay.app/api/v1/quick/add/user' \
--header 'x-token: jyrmRy2zH4EM5dfPH42m' \
--header 'Content-Type: application/json' \
--data-raw '{
   "email": "xxx@xxx.com",
   "name": "xxx",
   "password": "123456"
}'
```



&#x20; IV. Test Case\



We can use some test cases to test our team feature flow. In this process, I created 2 accounts for this test, the account information is as follows, we will use this information later to walk through the whole process.



Account A:

* email: testteamfeature\_b@footprint.network
* password: xPtW3G8xZhGzo7zyxcjH&#x20;
* userId: 24821
* project role: Manager
* project permission: view / manage / assign-user / remove-user / set-permission / add-dashboard / remove-dashboard\


Account B:&#x20;

* email: testteamfeature\_a@footprint.network
* password: xPtW3G8xZhGzo7zyxcjH&#x20;
* userId: 24820
* project role: Viewer
* project permission: view



Account C:

* email: testteamfeature\_ape@footprint.network
* password: xPtW3G8xZhGzo7zyxcjH&#x20;
* userId: 24826
* project role: Manager
* project permission: view / manage / assign-user / remove-user / set-permission / add-dashboard / remove-dashboard



Account D:&#x20;

* email: testteamfeature\_ape2@footprint.network
* password: xPtW3G8xZhGzo7zyxcjH&#x20;
* userId: 24827
* project role: Viewer
* project permission: view



Each of the following 5 steps will be demonstrated in turn.



1. Users can create projects via api
2. Assign users to the project
   1. This can be done via api or on TB
   2. Use the project Manager account for this on TB (I created a test account for this test with the following information)\


After logging in with this account in the staging environment, you can see the project's settings tab, where you can use the manager's permissions, user config, and dashboard config for configuration.



<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXc-5Y3axJDjiONXcxu9MUX_UIQLChGpZcWpUMF5Nsm9ZjlXpVmR91UV9jr1IL0885zXYY5DwixcmanDhWreiDxdb9882zv8o7TkMtNz8XCbzEItybGjTvY-_rYqNmt7qfQADX5ayERX41i9XVIBT7RHhBk?key=kW6BpWd1FahNqy-g-zFTHg" alt=""><figcaption></figcaption></figure>



Here in user config, we can configure 4 types of actions for the project.Add user to project, remove user from project, assign project role, and remove project role. In the Add User section, you need to fill in the user id to submit it.

\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXd-hiHpCjLqtcMlP4FkzzXDwU2pfEOz8qee1kox-yl0pcQFp3HKpBM03o6SH_VkSxcy29GI3z1iQlEvxL8Bw1xd7G6wmw0SzIIq1LJYw98JrTSsepID0kiEAH3oigPzRL1lnQEuWWBbLrvjHdsbwTYzhGY?key=kW6BpWd1FahNqy-g-zFTHg" alt=""><figcaption></figcaption></figure>



When the add user button is clicked, you can add a user to the project, just fill in the user id and submit it. Where do you get the user id from? (TBD， now just test with the user id of the given test account)\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXc59GmjbRvNnQvy1RzPFv0-524Ekol8dA6IFS_BLXw1jjcC0eBvqIF_qMoSRNoG0RRizD99kbztMIy-GPUvV0d6N7t9c7HDyhNpRDIV3CSZwNnnatLnL0XtQpy4QYnWZ0GGqXrUGr-6C59T-gaW1sK43OA?key=kW6BpWd1FahNqy-g-zFTHg" alt=""><figcaption></figcaption></figure>

When a user is added to a project, this project can be seen from the project list.\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfr4dyEBYB_4tmoDdRMBywDz9VKvO2TippH1WodWoPzbQDSwSIjZG2gExEx_V02enN6DLk1NQItIJQlJKmqBadSftjLvjTHw0S5qQFHYeiKXvY3iHlpZGJXQyAbxoOR41d9T9Os3fyzV3hgXA9rr9nkeII?key=kW6BpWd1FahNqy-g-zFTHg" alt=""><figcaption></figcaption></figure>



Once a user is removed from a project, that project will no longer be found in the project list.\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXefFZcT5F8DMWkohI4EBxbZhwHw1lG8NBWTE8Bc0dMgbp9p-XhIeiOGSmONbnbpnDwYfqNHWWrHzjxCeqbB22b2le4AQISKyVEm3UfM_kgXI11i1GukDGedGOAZCkzUSmqeZiz0nTp6mCHjHtd-l8fBIrwt?key=kW6BpWd1FahNqy-g-zFTHg" alt=""><figcaption></figcaption></figure>

3. Assign the project Manager role to the user

The project manager can assign/remove project roles to an account . Viewer is the default, so only Manager & Data User can be configured here.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXflPDIqfPc7A_ptefhCwlAi3m3u6xRyE51RugnhvlLz56rfSt9chsMrmBTDO-0BxJview38SaL6YCoN3VDH9y0staCTxOEXiyliITdVRm4Ww5Bz4J2Qz-Ud3sGSXEzJ0k0P5hWiFUaVvHFn7K2WJAuSbSM4?key=kW6BpWd1FahNqy-g-zFTHg" alt=""><figcaption></figcaption></figure>

4. Assign the dashboard into the project
   1. This includes two project actions: Add dashboard to project, remove dashboard from project.&#x20;

This shows all the projects inside the project.\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfHj0F8Pt-NYPq3dnm_N9EPKV0tIFsXpSLsi030qGPDVVJYqkHTh8ZlFiQMulySI5WgyM5RxS2LAYCnZFwPBuF6nIF8XF1HXmWBS1OH706xSPgReix-xYDeP74iEKBzaFufmXR_E8SIO7uABIOCbuUthX98?key=kW6BpWd1FahNqy-g-zFTHg" alt=""><figcaption></figcaption></figure>

In the Add Dashboard section, you need to fill in the dashboard id to submit it.（The dashboard id can be obtained by mouse over on the chart of the dashboard.）



<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXd08sFeb5A1CZ2uXXhEHjUe481zkF7ExjPeUVOi35554TGyQxK0ErR6s3cS7ORxaVMo6dHwl9VP6JyRa4karDUG5Zxlx0O9ZoTN_NZYOcmPnf0gQFvBPTKEZR1sI9_xl_hpOeiTiEA6UE0rgibxUQeID8i4?key=kW6BpWd1FahNqy-g-zFTHg" alt=""><figcaption></figcaption></figure>

When a dashboard is added to a project, the project's users can view the content of the dashboard



<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXe5Wouq3RZyPB5mR8qV9CTqJOIS06vwj0E4FwNco_9uEzPIrv3gPsdGHC-xI-7ifaEIMErXha4IfO1VlWA66gnBA0Ibc330Sts8F5t-brN6_pBdpzVe8c62MpiUkyICDb7JGmkYSCUnXIj4jHqrABx6_Irv?key=kW6BpWd1FahNqy-g-zFTHg" alt=""><figcaption></figcaption></figure>

When a dashboard is removed from a project, the project's users will no longer have the permission to view the content of the dashboard.



<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcC6mO60jHUgC7X8VtyrRDurEx-kDeoFfStI3ZphxRUvM5jsBFi5zKP7Vb9un40_FH29-rkn3GxLQd1A4kF8g07x-PTzWnMXCMHkUEmpbbdGttCJqE6_Hpkvsx_GbfXS9SapffoUtBQdQXD1usNBudX-i_b?key=kW6BpWd1FahNqy-g-zFTHg" alt=""><figcaption></figcaption></figure>

5. Last, we can check the permission of users in TB using test account\


