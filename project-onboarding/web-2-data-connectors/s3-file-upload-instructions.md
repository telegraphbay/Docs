# S3 File Upload Instructions

## S3 Connector Setup Guide

***

### 1. Grant Upload Permissions

#### Required Credentials:

* **Bucket Name**: `***`
* **Access Key**: `***`
* **Secret Access Key**: `***`
* **Region Name**: `***`
* **Root Directory Name**: `***`

> **Note**\
> Replace `***` with your actual credentials. Ensure proper IAM policies are attached for S3 bucket access.

***

### 2. Upload Path Rules

#### Directory Structure:

**First-level Directory**

* Path: `<Root directory name>/<table_name>`
* Example: For a table named `events`, the path is:\
  `s3://{bucket_name}/root_dir/events/`

**Second-level Directory (Optional)**

* Format: `p__on_date=YYYY-MM-DD`
* Example: For data dated `2024-01-01`, the path becomes:\
  `s3://{bucket_name}/root_dir/events/p__on_date=2024-01-01/`

> **Recommendation**\
> Use the second-level directory for better data organization and improved query performance. If omitted, the system reads all files under the first-level directory.

***

#### File Format Requirements:

* **Supported Formats**:
  * JSON
  * Parquet
  * CSV (Max 500,000 rows per file)

> **Important**\
> Files under the same table **must** use the same format.

***

#### Filename Convention:

* Ensure uniqueness to avoid overwriting.
* Suggested format:\
  `<table_name>_<start_time_range>_<end_time_range>.<format>`

**Example**:\
For hourly uploads of the `events` table:\
`events_2024010100_2024010101.json`\
This indicates data from `2024-01-01 00:00:00` to `2024-01-01 01:00:00`.

***

### 3. Create Tables with S3 Path Examples

#### With Second-level Directory (Partitioned)

```sql
CREATE TABLE raw_data.prod_test.portco_reposts (
  timestamp VARCHAR,
  text VARCHAR,
  type VARCHAR,
  p__on_date DATE WITH (
    partition_projection_format = 'yyyy-MM-dd',
    partition_projection_interval = 1,
    partition_projection_range = ARRAY['2020-04-01', 'NOW'],
    partition_projection_type = 'DATE'
  )
)
WITH (
  external_location = 's3a://{bucket_name}/{first_level_directory}/',
  format = 'JSON',
  partitioned_by = ARRAY['p__on_date'],
  partition_projection_enabled = true
);
```

#### Without Second-level Directory

```sql
CREATE TABLE raw_data.prod_test.portco_reposts (
  timestamp VARCHAR,
  text VARCHAR,
  type VARCHAR
)
WITH (
  external_location = 's3a://{bucket_name}/{first_level_directory}/',
  format = 'JSON'
);
```

#### CSV Format (Skip Header)

```sql
CREATE TABLE raw_data.prod_test.portco_reposts (
  timestamp VARCHAR,
  text VARCHAR,
  type VARCHAR
)
WITH (
  external_location = 's3a://{bucket_name}/{first_level_directory}/',
  format = 'CSV',
  skip_header_line_count = 1
);
```

> **Replace Placeholders**
>
> * `{bucket_name}`: Your S3 bucket name.
> * `{first_level_directory}`: Root directory name.

***

#### Additional Tips:

1. Use **partitioned tables** for large datasets to optimize query performance.
2. Validate file formats before uploading to avoid schema mismatches.
3. Enable versioning on your S3 bucket to prevent accidental data overwrites.

***

#### Usage Instructions:

1. Replace `{bucket_name}` and `{first_level_directory}` in code examples with actual values.
2. Ensure SQL syntax compatibility with your query engine (e.g., AWS Athena, Trino).
3. Strictly match time ranges in filenames with actual data timestamps.
