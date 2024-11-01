# S3 File Upload Instructions

## 1. Grant Upload Permissions,Examples Below

* Bucket name: \*\*\*
* Access key: \*\*\*
* Secret access: \*\*\*
* Region name: \*\*\*
* Root directory name: \*\*\*

## 2. The upload path follows these rules:

* First-level directory
  * Corresponds to the table name, for example, if you’re uploading a table named events, the directory path would be \<Root directory name>/events.
* Second-level directory
  * &#x20;Time field, when prefixed with p\_\_ equals the date, usually split by day, formatted as YYYY-MM-DD, for instance, if you want to upload data for the events table for the day of 2024-01-01, and your table’s time field is event\_time, then the directory path would be \<Root directory name>/events/p\_\_event\_time=2024-01-01.
* File format: json
* Filename:
  * &#x20;Can be customized, but ensure that your filename is unique to avoid overwriting existing files.&#x20;
  * If your filename matches an existing file in the bucket, the old file will be replaced by the new one.&#x20;
  * We suggest using <mark style="background-color:yellow;">\<table\_name>\<start\_time\_range>\<end\_time\_range>.json</mark>,
  * for example, if you upload data for the events table hourly, the filename should be: \<Root directory name>/events/event\_time=2024-01-01/events\_2024010100\_2024010101.json. &#x20;
  * This filename indicates it contains data from the time period 2024-01-01 00:00:00 to 2024-01-01 01:00:00, to facilitate the most cost-effective data updates.\
    \
