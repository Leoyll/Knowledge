## index is not used
```sql
SET @deleteDate = '2021-11-22';    #修改
 
#delete from tb_commission_ib_transaction where commission_ib_id in (select id from tb_commission_ib where close_time >@deleteDate);

# If the number of rows of tb_commission_ib_volume is huge, and the SQL is pretty slow.

delete tcit.*
from tb_commission_ib_transaction tcit
join tb_commission_ib tci on tci.id=tcit.commission_ib_id
where tci.close_time > @deleteDate;

# It will use index_close_time of tci and commission_ib_id_index of tciv
```

## index
If the correct index is created, but SQL query does call the correct index.
```sql
explain tableName;  #show the table index
explain SQL query;  #show the index usage for the query
ANALYZE TABLE tableName;  #MySQL uses the stored key distribution to decide the table join order for joins on something other than a constant. 
                          #In addition, key distributions can be used when deciding which indexes to use for a specific table within a query.
                          #Use it can improve performance.
```
### ANALYZE TABLE vs OPTIMIZE TABLE 
1. OPTIMIZE TABLE  
   a) reorganizes the physical storage of table data and associated index data, to reduce storage space and improve I/O efficiency when accessing the table.<br>
   b) OPTIMIZE TABLE copies the data to a new tablespace, and rebuilds indexes. This takes a long time for a large table.<br>
2. Analyze table <br>
   a) performs a key distribution analysis and stores the distribution for the named table or tables.<br>
   b) Analyze table doesn't take a long time, and doesn't take longer for a large table. <br>
   The way analyze table helps performance is that it updates statistics that the optimizer uses to choose indexes for a given query. <br>
   If the updated statistics don't make any material difference to the choice of index, it won't have any effect on performance. <br>
   It only matters if the new statistics would result in the optimizer choosing a more favorable index.<br>
