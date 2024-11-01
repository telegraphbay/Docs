---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Quality Control Procedure

Quality control procedure in Telegraph Bay is demonstrated in the following UML, with 2 key users of Project User and TB Admin（TB refers to Telegraph Bay data project afterwards.）&#x20;

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXesh487EgLFHYb2TMf5z1PKue7LIIsa0TOHON_T6BFF9bQLI1W8fA6Tl5CPzMcUSljllbIGzXonyS8e6orD7Ie2EZBisjN-KMir7q2b4WLNZztHVXx49ovP9a1hKPeoc5OxuwC_zYDZWYtXKgFUaIVNssWj?key=eb3pMwPYPcjLQyC-gze8Ow" alt=""><figcaption></figcaption></figure>

( Click this [png](https://www.plantuml.com/plantuml/png/jLF1RXGn3BtFLrWz5mvmMsbHeKK8gKYbb1iNJ-AT3Sqa8N5sjN\_Fn9CPKfOA2ObRP3y\_lnVRLuanSXvjnuOjmUVhhkjGiOzmSHVzTr8CNnF52y048NVAMpl5Z7S9BcwG4UufUCdgk1G-l0FKet4IPjtrFZkDyIIP6qpxtcFKDTi\_JIll70dD5phKlgnIfv6nnqG2-gYefpZCITFa5iatDJBBJbsttS87m\_lSmxLtEpFaYCK4RBTToF3Y3R3UG8Z-O3J1C2DHgKaPTwmN0APa1hVa8mKFWU9efh1tfKVj\_rmJ7JTWGFkZinurC8t18XCeWLTGo1lO6MVIliR1k9sFuynbpIE15d6KagGkuPkRa1nHSRg4zm-tOFrWrFIwVjdOI-Gqy9uMsfstcYAmXpemiJ2ztvRtcOVc7vOK2LsefXkrW4uBwzClL6a3J8cD6neFtHIlp6isPScrXNKpfD1faCM5T79ghYy\_Dw46OeXuXB8Jq9yW-HmLDMirgrnhs6mWuO64gP1oJiBFPgJ4E8QgGSSYOHmDDpzSoNHSLUQWVqsq25YMIOvcjQf4giDOm2E6C9ckdEc7BNMDpYgJs1Q1vLOqCFQM9DkQ4CZp7bZtauazgKVvdmP-3bWxwULWAwRif\_DiTgUAN07IKqoqjx0Ul5nbhMdlxep6k6btclp1q16ErB-QLJEcHx0cyHyWht-3526VYADHS5k4jUsNgKOgrSrKPRroNqQDS7TVVJVG40KpKXpeFmuVS43ZpfGsF\_KFUjj9LxtdewvJd40LM\_Uh2Kk7lrDSaTDvj3y1) to see the full size image)



## 1. Github Configuration

* Project User first provides a github account to TB Admin, after permission assigned to the [github repository](https://github.com/telegraphbay/data-model/), Project User can submit project configuration.



## 2. Project Configuration

* When the mailbox receives an email that the permission has been granted, Project User can do the following
* The Project User creates the project folder by using template config files.&#x20;
* Git clone from [github repository](https://github.com/telegraphbay/data-model/)
* Create a folder in projects directory
  * folder name in uppercase, followed by a hyphen,  and the project name. For example, Project-A
* Copy the template config folder to the newly created folder. The template config files are available in the [link](https://github.com/telegraphbay/data-model/tree/main/config).

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfWZ2FjNoMHQzNfkLC3QGxbPS_PDhAkub4oXijQxYdg2Giu8RTvyVZNVD3UaEPr1wJyaskOWuX94P91xB-T1HPNK-voFI05MtYf7GSHfrp_YVHWY9AvwNbALpFXNjlFFGAUzQP1SholoWTsO_cPpo7OBSts?key=eb3pMwPYPcjLQyC-gze8Ow" alt=""><figcaption></figcaption></figure>



* Complete the content filling in the project folder:
  * Data source configuration
    * Fill in the connector details and&#x20;
  * Web3 static data configuration
    * Provide contract address of NFT or Token
    * Provide wallets address as of NFT primary sales & royalties
  * ETL logic configuration
    * Choose either sql or yml folder, and complete the ETL logic writing.&#x20;
      * For developers, it is recommended to use SQL to execute ETL logic, as it is efficient in handling data operations.&#x20;
      * For non-developers or those who are not familiar with SQL, YAML format can be used as an alternative. YAML is an intuitive and readable data serialization format that allows these users to define ETL tasks in a user-friendly way. By creating YAML files, they can specify parameters and steps for the ETL process without needing to understand the intricacies of SQL syntax.
* TB account configuration
  * Provide a Google account to login to TB
* After completing the filling, send the config folder to TB admin via slack.



## 3. TB Admin Configuration&#x20;

* Admin will configure the project, synchronize the raw data of user\_event/accoun\_mapping and add the TB account for Project User.&#x20;

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeUv4MJtJBM_dSUwUL_5dzyk6qmXA5J3EXGu9jzEv4-xkUpEfikYP-6R4wkO6XcDW5QcpH4USkYnxMFyPbYBwGcDGhCRCsbhB6vrGF9rjqjHajsdMtzMYzImMQHXSuTj25PdFxUQlxjDwHktru5dF0mXkzG?key=eb3pMwPYPcjLQyC-gze8Ow" alt=""><figcaption></figcaption></figure>

* After the TB account is created, Project User will be able to login TB to see the dashboard from the link ([TB](https://www.telegraphbay.app/))  through the onboard success email.



## 4. Project Dashboard Data Check

Once the project is onboarded successfully and the project user already has an account in TB, the Project User can look at the reports in TB and analyze them.

* Dashboard data
  * Project User can see the dashboard in TB.
* Web2 data task list
  * Project User checks the table sync status report at the [link](https://www.staging.telegraphbay.app/ab/dashboard/@Bond/QC-Job-Status-Dashboad).

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeS4EhdXodr6amueHZT_MsUXF3Pflg50Qf4Kh1Br3djFbe4aRH5Jg7LPlQZgT5vfdsrVSUMR18zfuk7TK8_AdQd8l4vq_Nsr8r_U4rdajnhnEgtUdZZXeAZaYpbC3ItPT3fuoZYU6rTiPCc-GcxHtGgczMM?key=eb3pMwPYPcjLQyC-gze8Ow" alt=""><figcaption></figcaption></figure>

* Project User review raw table data:
  * Access the SQL chart to query in TB at [link](https://www.staging.telegraphbay.app/ab/chart#eyJkYXRhc2V0X3F1ZXJ5Ijp7ImRhdGFiYXNlIjpudWxsLCJuYXRpdmUiOnsicXVlcnkiOiIiLCJ0ZW1wbGF0ZS10YWdzIjp7fX0sInR5cGUiOiJuYXRpdmUifSwiZGlzcGxheSI6InRhYmxlIiwicGFyYW1ldGVycyI6W10sInZpc3VhbGl6YXRpb25fc2V0dGluZ3MiOnt9fQ==).
  * Selecting the raw table to view data, for example, you can choose to check the timestamp of the last record in the data. For example, use "select max(datetime) from table."

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXc1qC3Su3U3fOzcruiRDMUz0_15kui3R-wwL3CJF6DgAqPg2X6BRvdSVlpbItbfaxNq_cS5Kt74Cz3ZP8OXn1IZS3GKg04NHfLEECzTRLixgN_AW_p1yoUGvez4Q5s2g_62XMygrSqK-8VcrWLMh70QAvIj?key=eb3pMwPYPcjLQyC-gze8Ow" alt=""><figcaption></figcaption></figure>

* Web3 data task list
  * Project User check the web3 quality dashboard:
  * Detecting the consistency of data between TB and FP in the [link](https://www.footprint.network/@Bond/Ani-indicator-dashboard).\


## 5. Metric Logic Check

* Project User review dashboard and ETL logic in GitHub repo.
  * Dashboard logic is in the [link](https://github.com/telegraphbay/data-model/projects/%7B%7Bproject%7D%7D/Dashboard-Logic)

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeBjfO182t3hIdbnOpxAZZghqsRdIioqep-5GHEXwmAxb894PaRCRbqa6ZP9ZDi4fLDFB0tolpfJcv9_uXMCdm86dnJEKr3dA__-l3Gt2GoTWcwh2_bcS5XTFp8XvqaLs8t8V-eshAV05u08XLbrYBC0UIK?key=eb3pMwPYPcjLQyC-gze8Ow" alt=""><figcaption></figcaption></figure>

\


* ETL logic is in the [link](https://github.com/telegraphbay/data-model/projects/%7B%7Bproject%7D%7D/ETL-Logic)

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXflOBxzgnnYRxdlr1D6tE3YaNkNMO3Ds4jCEqZksRO3rxIW2FMRfzPWn7j80nsK97UC1rCDsXkVWmS_YCz0tAGhQYbPU_Km5xeNOhhtH898xsY7ObkXe7Zf2HAsaZaQc05TBVsAz2kAqTCc34lBhqWJjXLP?key=eb3pMwPYPcjLQyC-gze8Ow" alt=""><figcaption></figcaption></figure>

## 6. Issue Feedback

* Contact the TB admin for data issues during the check.
* Submit a PR to the GitHub repository for dashboard or ETL logic concerns, awaiting admin review and merge.
