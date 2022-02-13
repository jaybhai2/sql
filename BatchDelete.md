## bulk delete run for a long time?
#### remove all foreign key relationship, remove all trigger , remove all on delete cascade

```

ALTER TABLE table_name DISABLE TRIGGER trigger_name | ALL
DELETE from ....
ALTER TABLE table_name ENABLE TRIGGER trigger_name | ALL

```

#### do batch delete instead
##### rerun until not row are return by the cte
```
WITH delete_scope AS (
   SELECT id                 -- your PK
   FROM   tbl
   WHERE  date < $something  -- your condition
   -- ORDER BY ???           -- optional, see below
   LIMIT  50000              -- number of batch
   FOR    UPDATE             -- SKIP LOCKED ? If FOR UPDATE or FOR SHARE is specified, the SELECT statement locks the selected rows against concurrent updates.
   )
   
DELETE FROM tbl
USING  delete_scope
WHERE  tbl.id = delete_scope.id;


```
