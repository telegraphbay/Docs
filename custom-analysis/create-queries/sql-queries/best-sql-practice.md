# Best SQL Practice

## I. Basic Tips for SQL Queries

### 1. Query Assets for a specific Project

Here in TB there are three tables for this usage:

* `project_mapping`
* `nft_collection_info`
* `token_info`\


SQL demo examples:&#x20;

* NFT Collections

```sql
select
chain
, contract_address 
from nft_collection_info 
where protocol_slug in (
                            select 
                            protocol_slug 
                            from project_mapping
                            where project = {{project}}
                        )
```

* Token Asset

```sql
select
chain
, token_address 
, token_name
, token_slug
from token_info 
where protocol_slug in (
                            select 
                            protocol_slug 
                            from project_mapping
                            where project = {{project}}
                        )
```

### 2.  Query Assets for a specific Project

Here in TB there are a few of tables for this usage:

* balance by block
  * `address_token_balance`&#x20;
  * `address_nft_balance` &#x20;
* latest balance&#x20;
  * `address_token_latest_balance`
  * `address_nft_collection_latest_balance` &#x20;
* daily balance
  * `address_token_daily_balance` &#x20;
  * `address_nft_daily_balance` &#x20;



Three major scenario:

1. Latest Balance:

* NFT Latest Balance&#x20;

```sql
select
wallet_address
, collection_contract_address
, sum(amount) as amount
from (
        select 
        *
        from address_nft_latest balance
        where  collection_contract_address =  {{collection_contract_address}} 
         ) 
where amount > 0
group by 1,2
```

* Token Latest Balance

```sql
  select
  wallet_address
  , amount
  from  address_token_latest_balance
  where token_address = {{token_address}}
  and amount > 0
```

2. Daily Holders:

* NFT Daily Holders

```sql
select 
on_date  as "Date"
, count(distinct wallet_address) as "# Holders"
from address_nft_collection_daily_balance  
where 1= 1 
and on_date >= date'2024-01-01'
and  collection_contract_address = {{collection_contract_address}}
and holding >0
group by 1
    
```

* Token Daily Holders

```sql
with token_list as (
    select token_address, chain from token_info 
    where token_symbol = {{toke_symbol}}
    [[ and  {{chain}}]]
    )
, holding_balance as (
    select 
    on_date
    , wallet_address
    ,  holding  
    from address_token_daily_balance
    where 1 = 1 
    and token_address in (select token_address from token_list)
    and holding >0
    and on_date >= date'2024-01-01'
    )
select 
on_date 
, count(distinct wallet_address ) as holders
from holding_balance
group by 1
```

3. Snapshot Balance

* NFT Snapshot Balance

```sql
with t as (
    select 
    *
    , row_number() over (partition by chain, wallet_address, nft_token_id order by block_number desc) as rn 
    from address_nft_balance  
    where 1 = 1 
    and collection_contract_address = lower( {{collection_contract_address}})
    [[and block_number <= {{block_number}}]] -- get holding number until this block
    [[and block_timestamp <= {{block_timestamp}}]]
)

select 
*
from t  
where 1 = 1 
[[and nft_token_id = {{nft_token_id}}]]
and rn = 1
and amount > 0
order by block_number desc

```

* Token Snapshot Balance

```sql
with t as (
    select 
    *
    , row_number() over (partition by chain, wallet_address order by block_number desc) as rn 
    from address_token_balance  
    where 1 = 1 
    and token_address = lower( {{token_address}})
    [[and block_number <= {{block_number}}]] -- get holding number until this block
    [[and block_timestamp <= {{block_timestamp}}]]
)

select 
*
from t  
where 1 = 1 
and rn = 1
and amount > 0 
order by amount desc

```

### 3. Query Linked Wallet Users & NFT Holding

Here in TB there are a few of tables for this use

* `account_mapping`
* `project_mapping`
* `nft_collection_info`
* `address_nft_balance`

Sql demo example:

* [Users linked wallets as NFT Holders](https://www.telegraphbay.app/chart/Best-Practice-Demo-Query-Users-Linked-Wallet-as-NFT-Holder-fp-44592)



### 4.Query Token Moneyflow

Here in TB there are a few of tables for this usage

* `address_attributes`
* `mf_address_cpty_token_stats`

Sql demo example:

* [Token Moneyflow to DEX](https://www.telegraphbay.app/chart/Best-Practice-Demo-Query-Token-Moneyflow-to-DEX-fp-44593?token\_symbol=GMEE\&on\_date=past30days\~)



Please pay attention to the direction of the moneyflow. If you want to query the money flow to the address tagged as “DEX”, please use the interact\_address  when join with the address\_attributes.



\


## II. Intermediate Tips for SQL Parameters

In TB , there are multi-ways to add filters. You can simply use the \{{filter\_name\}} in your where condition.



1. Single selection
   1. If your query condition is as simple as one filter, you can just use like:  where token\_symbol = \{{token\_symbol\}}, then you can select the variable type as ‘string’ and set a value as default.
   2. If your query condition is not required, you can rewrite as  where 1= 1 and  \[\[ token\_symbol = \{{token\_symbol\}}]] &#x20;
   3. If your query condition is a number type, remember to change the variable type as “Number”
2. Multiple selection
   1. If your selection is more than one, for example, you want to query two or more tokens, you can set your variable type as  ‘field filter’ by changing the condition as where 1= 1 and  \[\[  \{{token\_symbol\}}]]  .
   2. This required the table as a standard table already shown on the menu.
3. Date Range Selection
   1. Since data selection is the most common case in TB. It is recommended to use the above ‘Multiple Selection’  as the way to add a date filter.

More explanation on SQL variables can be found here [in this document.](https://www.metabase.com/docs/latest/questions/native-editor/sql-parameters.html)



## III. Advanced Tips for SQL Optimization

Tips for sql optimization:&#x20;

1. Use of Pre-generated Date Table
   1. Since date-range selection is of high-frequency in TB of most dashboards, we have created a table  select \* from iceberg.prod\_struct.day\_list  for date selection
   2. This table helps to solve 50% slow query issues.
2. Reduced unnecessary column selection, avoiding use ‘select \* from‘
3. Early deduplication to reduce data amount\


Take the chart of DAU, MAU, and Peak Concurrent by Day as Example explaining tips #1,#2 and #3

* [Previous sql](https://www.telegraphbay.app/chart/DAU%2C-MAU%2C-and-Peak-Concurrent-by-Day-before-optimized-fp-44522?project=Wreck%20League\&timestamp=past180days\~)
* [Optimised sql](https://www.telegraphbay.app/chart/DAU%2C-MAU%2C-and-Peak-Concurrent-by-Day-fp-43918?project=Wreck%20League\&timestamp=past180days\~)\
  \
  \
