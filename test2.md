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
```sql
create database if not exists problem4;
use problem4;

CREATE EXTERNAL TABLE employee1(customer_id INT, fname STRING, lname STRING, address STRING, city STRING, state STRING, zip STRING)
ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t' 
STORED AS TEXTFILE 
LOCATION '/user/training/problem4/data/employee1';

CREATE EXTERNAL TABLE employee2(customer_id INT, none STRING, fname STRING, lname STRING, address STRING, city STRING, state STRING, zip STRING)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
STORED AS TEXTFILE 
LOCATION '/user/training/problem4/data/employee2';

CREATE TABLE solution 
ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t' 
STORED AS TEXTFILE 
LOCATION '/user/training/problem4/solution' AS 
SELECT customer_id, initcap(fname) as fname, initcap(lname), address, city, state, substr(zip,1,5) from employee1
UNION ALL
SELECT customer_id, initcap(fname) as fname, initcap(lname), address, city, state, substr(zip,1,5) from employee2;
```

Problem5
```sql
# solution.sql

mkdir problem5
-- solution.sql

use problem5;

select concat_ws('\t', c.fname, c.lname, c.city, c.state)
from customer c
where c.city = 'Palo Alto'
and c.state = 'CA'
union all
select concat_ws('\t', e.fname, e.lname, e.city, e.state)
from employee e
where e.city = 'Palo Alto'
and e.state = 'CA'
;
```
Problem6
```sql
create table solution row format delimited fields terminated by '\t'
stored as textfile as
select id, 
       fname, 
       lname, 
       address, 
       city, 
       state, 
       zip, 
       substr(birthday, 0, 5) 
from employee;
```
Problem7
```sql
select concat(fname, ' ', lname) as name 
from employee 
where city='Seattle' 
order by fname;
```

Problem8
```
sqoop export \
--connect jdbc:mysql://localhost/problem8 \
--dirver com.mysql.jdbc.Driver \
--username cloudera \
--password cloudera \
--table solution \
-m 1 \
--export-dir '/user/training/problem8/data/customer/' \
--fields-terminated-by '\t' \
--columns 'id, fname , lname , address , city, state , zip , birthday';
```
Problem9
```sql
create table solution 
select concat('A', id) as id, fname, lname, address, city, state, zip, birthday from customer;
select * from solution;
```

Problem10
