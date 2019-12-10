# MySQL的一些杂项配置(sql_mode)

**引入问题**

执行下面的sql

```sql
CREATE TABLE mytbl ( id INT, NAME VARCHAR ( 200 ), age INT, dept INT );
INSERT INTO mytbl VALUES( 1, 'zhangsan', 33, 101 );
INSERT INTO mytbl VALUES( 2, 'lisi', 34, 101 );
INSERT INTO mytbl VALUES( 3, 'wangwu', 34, 102 );
INSERT INTO mytbl VALUES( 4, 'zhaoliu', 34, 102 );
INSERT INTO mytbl VALUES( 5, 'tianqi', 36, 102 );

#每个机构年龄最大的人
SELECT name, dept, MAX(age) FROM mytbl GROUP BY dept;
```

报错信息

```txt
SELECT name, dept, MAX(age) FROM mytbl GROUP BY dept
> 1055 - Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'mydb2.mytbl.NAME' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
> 时间: 0.036s
```

原因：mysql5.7 较之前版本设置了`sql_mode` ,其中有一项为`only_full_group_by`，这个设置规定select后只能查询`函数`中的字段和`group by`之后的字段。

**正确查询：**

```sql
SELECT * FROM mytbl m INNER JOIN(
SELECT dept, MAX(age)maxage FROM mytbl GROUP BY dept) ab
ON ab.dept = m.dept AND m.age = ab.maxage;
```

**查看数据库的sql_mode**

```sql
SHOW VARIABLES LIKE 'sql_mode';
```

![](https://zsy0216.github.io/image//notes/20191210190120.png)