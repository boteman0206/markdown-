### 备份数据库

````mysql
mysqldump -h localhost -uroot -p1234 --databases login  > C:\Users\Administrator\mysqldump\backup.sql
````

**如果windows的权限不足，请注意备份使用的路径**

