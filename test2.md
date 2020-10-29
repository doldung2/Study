Problem1
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

Problem2
```sql
create database if not exists problem2;

use problem2;

create external table if not exists solution(
  id int,
  fname string,
  lname string,
  address string,
  city string,
  state string,
  zip string,
  birthday string,
  hireday string
  )
  row format delimited fields terminated by '\t'
  stored as parquet location "/user/hive/warehouse/employee"
```
```linux
dfs -cp /user/training/problem2/data/employee/*.parquet /user/hive/warehouse/employee/
```

Problem3
```sql
use problem3;

create external table if not exists solution(
  id int,
  fname string,
  lname string,
  hphone string
  )
  row format delimited fields terminated by '\t'
  stored as orc location "/user/training/problem3/solution"
  
insert into solution
select c.id
, c.fname
, c.lname
, c.hphone
from customer c, account a
where c.id = a.custid
and a.amount < 0;
```
Problem4
Problem5
Problem6
Problem7
Problem8
Problem9
Problem10
