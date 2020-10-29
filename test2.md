```sql
-- solution.sql

use problem1;

select a.id as id,
       a.type as type,
       a.status as status,
       a.amount as amount,
       round(a.amount - b.average,2) as difference
from account a,
     (select type, avg(amount) as average from account group by type) b
where a.status='Active'
and a.type = b.type
;
```
