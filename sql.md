## Postgres

#### Select an empty array

```sql
select array[]::varchar[] as "labels";
```

## SQL Queries

#### Comma list of multiple rows

```sql
-- Would like to understand how to order those sub queries
select
  b.guid,
  (
    select
      string_agg(name, ',')
    from
      batch_status_log bsl
      inner join batch_status bs on bs.id = bsl.status_id
    where
      bsl.batch_id = b.id
  ) as "batch_statuses",
  (
    select
      string_agg(name, ',')
    from
      task_status_log tsl
      inner join task_status ts on ts.id = tsl.status_id
    where
      tsl.task_id = t.id
  ) as "task_statuses"
from
  batch b
  inner join task t on b.id = t.batch_id
where
  guid = '235b2594c0a649b9a92652aece9eaa1d';
```

#### Join a specific row in a one to many relationship

Finds the most recent "child" based on a sub query.
In this case, a `batch` has many entries in a `batch_status_log` table.
Our sub query finds the `max` date for the given batch and then
joins back to that same record to make it available to the outer query.

```sql
select
  batch.guid as "batchGuid",
  batch.created_ts as "created",
  fred_user.email as "email"
from batch
inner join (
          select B.batch_id,B.status_id,B.created_ts
          from  (
              -- Find the most recent batch status based on created_ts value
              select batch_id,max(created_ts) as created_ts
              from  batch_status_log
              group by batch_id
          ) A
          -- join back to batch_status_log to get the particular status above
          join batch_status_log B on A.batch_id=B.batch_id and A.created_ts=B.created_ts
          where B.status_id=10 -- CHANGE STATUS ID!
      ) bsl
on batch.id = bsl.batch_id
inner join task on batch.id = task.batch_id
inner join fred_user on batch.created_by = fred_user.id
order by batch.created_ts;
```
