# Quick Start Guide to Trino

## 1.Connecting to Trino

Before you begin, you will first need to obtain the Trino host, user\_name, and password,and prepare your client (using [DBeaver](https://dbeaver.io/download/) as an example here)



Create New connection -> Choose Trino -> Config connect -> Test connect -> Save connect -> New SQL editor -> Start query

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeNu_X0QFlGnddtzX_HlwqbG0lJGhHHoEbsRySyMh_YoBGvD9_cAKvjmGykVRkRgdzcZnU3kHs7qutkTiMjY5FHq4-caugEx3LXFbQP4OgUsRMbPnwb7UPK8hg0VUzJm2-fMsTYpAMYYiOmdNsMZpEMvMY?key=MbmiDaVj-OsfFdvkiV9dkA" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXd86tatjSDhBOe8wGXcM6j0_Bkurm5mwy31mOryU9nWNPXS52SYEfeObC85Cdz3RDteGLyFAis-jxRVYrGv9vrZOMcR1EkpIHovQP4Vxa8_o92RQ36q7tkjd4GinevoQFo87IZ-RXPH8QOrbqUrmXNxQ7uU?key=MbmiDaVj-OsfFdvkiV9dkA" alt=""><figcaption></figcaption></figure>

## 2. Create table with Iceberg connector

When the data source is from Iceberg, or when data is inserted one by one, you can use the Iceberg connector to create a table on Iceberg.



create table:

```sql
CREATE TABLE iceberg.{{schema}}.example_table (
    c1 INTEGER,
    c2 DATE,
    c3 DOUBLE
)
WITH (
    format = 'ORC',
    partitioning = ARRAY['c1', 'c2'],
    sorted_by = ARRAY['c3']
)
```

insert into table one by one:

```sql
insert into iceberg.{{schema}}.example_table 
select 1 as c1, date '2024-06-24' as c2, 1.0 as c3
```

insert into table with query database:

```sql
insert into iceberg.{{schema}}.example_table 
select c1, c2, c3 from iceberg.{{schema}}.example_table_2
```

update data:

```sql
update iceberg.{{schema}}.example_table  set c3=0.2 where c1=1
```

delete data:

```sql
delete from iceberg.{{schema}}.example_table where c1=1
```

drop table:

```sql
delete from iceberg.{{schema}}.example_table where c1=1
```



Supplementary Information:

1. Tables created via the Iceberg connector have the permissions to delete and modify data, for more detailed instructions, please refer to: [https://trino.io/docs/current/sql.html](https://trino.io/docs/current/sql.html).
2. Partitioning and sorted\_by can optimize the query speed of a table. Here’s how to use them: [https://trino.io/docs/current/connector/iceberg.html#partitioned-tables](https://trino.io/docs/current/connector/iceberg.html#partitioned-tables)



## 3. Create table with Hive using S3 locations

When files are uploaded to S3 in file format, you can create a table using this method.\


1. The S3 file directory is not partitioned, for example: ‘s3a://data/example\_table/\*.json’

```sql
c1 INTEGER,
    c2 DATE,
    c3 DOUBLE
)
WITH (
   external_location = 's3a://data/example_table',
   format = 'JSON'
)
```



2. The S3 file directory is partitioned, for example: ‘s3a://data/example\_table/p\_\_created\_date=yyyy-MM-dd/\*.json’. Directory partitioning helps to improve query efficiency

```sql
CREATE TABLE raw_data.{{schema}}.example_table_with_no_partition (
    c1 INTEGER,
    c2 DATE,
    c3 DOUBLE,
    p__created_date date WITH ( partition_projection_format = 'yyyy-MM-dd', partition_projection_interval = 1, partition_projection_range = ARRAY['2011-01-01','NOW'], partition_projection_type = 'date' )
)
WITH (
   external_location = 's3a://data/example_table',
   format = 'JSON',
   partition_projection_enabled = true,
   partitioned_by = ARRAY['p__created_date']
)
```



The query is consistent with the one mentioned above.

Tables created by mounting S3 through Hive do not have the permissions to delete or modify data.\
\
However, for tables created by mounting S3 through Hive, you can perform a “drop table” action. After dropping the table, the original files will not be deleted.

## 4. Query in TB

1. If you want to query on TB, you only need to provide the full path for the table being queried. For example, to query `example_table_with_no_partition`, you can simply query in TB’s SQL Query editor:&#x20;

```sql
select * from raw_data.{{schema}}.example_table_with_no_partition
```

2. If you want to make the table accessible in the left menu bar, please wait for Duke to complete the documentation.

## 5. Relevant Documentation

1.[https://trino.io/docs/current/connector/iceberg.html](https://trino.io/docs/current/connector/iceberg.html)

2.[https://trino.io/docs/current/connector/hive.html#hive-connector](https://trino.io/docs/current/connector/hive.html#hive-connector)\
\
\
