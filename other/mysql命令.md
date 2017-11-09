# mysql命令

## 操作数据库

显示所有数据库：

    SHOW DATABASES;

创建数据库：

    CREATE DATABASE mydb;

创建数据库并指定编码：

    CREATE DATABASE mydb DEFAULT CHARACTER SET UTF8;

删除数据库：

    DROP DATABASE mydb;

使用数据库：

    USE mydb;

## 操作表

显示数据库中所有表：

    SHOW TABLES;

创建表：

    CREATE TABLE mytable(
      id INT,
      name VARCHAR(4)
    )

字段名在前，类型在后，括号内的数字表示最大长度

增加一列：

    ALTER TABLE mytable ADD COLUMN age INT;

修改已有列：

    ALTER TABLE mytable CHANGE name bname VARCHAR(12);

注意：如果表中有数据时，在修改列时可能会失败，比如表中存有一个长度为4的name，此时如果将name的最大长度改为3，则会失败

删除指定列：

    ALTER TABLE mytable DROP name;

此列的所有数据均被删除

重命名表：

    ALTER TABLE mytable RENAME TO newtable;

重命名表不会删除表数据

### 删除表

    DROP TABLE mytable;

## 表数据操作

### 插入数据

向指定列插入数据：

    INSERT INTO mytable(id,name) VALUES(1,"tom");

如果要向所有列插入数据，可以用下面的方式进行简写，注意用这种方式插入数据，必需要按照列的顺序插入：

    INSERT INTO mytable VALUES(2,'jack');

### 查询数据

查询所有列的数据：

    SELECT * FROM mytable;

*号表示所有列

查询指定列的数据：

    # 只查询name与age的数据
    SELECT name,age FROM mytable;

    # 给列指定别名
    SELECT name AS "姓名",age AS "年龄" FROM mytable;

按条件查询：

    # 单条件查询
    SELECT * FROM mytable WHERE id=1;

    # AND表示并且
    SELECT * FROM mytable WHERE age=12 AND name="tom";

    # OR表示或者
    SELECT * FROM mytable WHERE age=12 OR name="tom";

    # IN表示一个区间，age满足括号内其中一个即可
    SELECT * FROM mytable WHERE age IN (12,15,20);

    # age小于15
    SELECT * FROM mytable WHERE age<15;

    # age介于10到20之间
    SELECT * FROM mytable WHERE age BETWEEN 10 AND 20;

### 修改数据

    # 将tom的age改为13
    UPDATE mytable SET age=13 WHERE name="tom";

    # 同时修改多个列
    UPDATE mytable SET age=13,gender="male" WHERE name="tom";

### 删除数据

删除所有数据：

    # 第一种方法
    DELETE FROM mytable;

    # 此方法也可以删除表中所有数据，相对于第一种方法，有三个特性：
    # 1.效率更高；
    # 2.可以清除自增长；
    # 3.只能用于删除全部数据，不能带条件删除
    TRUNCATE TABLE mytable;

根据条件删除数据：

    # 删除id为1的数据
    DELETE FROM mytable WHERE id=1;

## 查询进阶

分页查询：

    # 查询表中前5条
    SELECT * FROM mytable LIMIT 5;

    # 从第5条开始（第一条为0），查询10条
    SELECT * FROM mytable LIMIT 5,10;

排序查询：

    # 升序查询
    SELECT * FROM mytable ORDER BY age ASC;

    # 降序查询
    SELECT * FROM mytable ORDER BY age DESC;

模糊查询：

    # %表示任意多个字符，_表示一个字符
    SELECT * FROM mytable WHERE name like %tom%;

null与非null查询：

    # 查询name为null的数据
    SELECT * FROM mytable WHERE name IS NULL;

    # 查询name为非null的数据
    SELECT * FROM mytable WHERE name IS NOT NULL;

聚合函数查询：

    # 查询总记录数
    SELECT COUNT(1) FROM mytable;

    # 查询表中最大的age
    SELECT MAX(age) FROM mytable;

    # 查询表中最小的age
    SELECT MIN(age) FROM mytable;

    # 查询表中age的平均值
    SELECT AVG(age) FROM mytable;

分组查询：

    # 查询各个age拥有的数量
    SELECT COUNT(1),age FROM mytable GROUP BY age;

| count(1) | age |
| -------- | --- |
|    1     |  14 |
|    2     |  15 |
|    1     |  16 |

    # 查询各个age拥有的数量，只显示数量>1的
    SELECT COUNT(1),age FROM mytable GROUP BY age HAVING COUNT(1)>1;

## 数据类型

INT(n)：整型，默认最大存储9位数。

注意n并不是指最大长度，而是指配合ZEROFILL使用时进行0填充。

    CREATE TABLE mytable(
        id INT(4) ZEROFILL;
    )

    INSERT INTO mytable(id) VALUES(2);

|  id  |
| ---- |
| 0002 |

CHAR(n)：字符，默认长度为1，如果指定了n，则长度为n。

varchar(n)：字符串，n表示最大长度，创建表时必须指定n，不然会报错。与char不同的时，char指定n后，如果插入的数据不足n位，那么数据库中还是会占用n位的空间，varchar指定n后，插入的数据是多少位，那么数据库中就占用多少位的空间。

ENUM：枚举，表示n选1，插入数据时只能从指定的值中选一个，不然会报错。

    CREATE TABLE mytable(
        country ENUM('English','America','China')
    );

    # Query OK
    INSERT INTO mytable VALUES('China');

    # ERROR
    INSERT INTO mytable VALUES('Japan');

SET：多选，表示n选n，插入数据时可以从指定值中选一个或者多个插入，用“,”隔开。

    CREATE TABLE mytable(
        country ENUM('English','America','China')
    );

    # Query OK
    INSERT INTO mytable VALUES('China,English');

|    country    |
| ------------- |
| China,English |

日期类型：

    # DATE：日期
    # DATETIME：日期+时间
    # TIMESTAMP：时间戳，日期+时间，该条记录创建或者修改的时间，自动插入
    CREATE TABLE mytable(
        d1 DATE,
        d2 DATETIME,
        d3 TIMESTAMP
    )

    INSERT INTO mytable(d1,d2) values('2017/11/9','2017/11/9 23:45:33');
    INSERT INTO mytable(d1,d2) values('2017-12-9','2017-12-9 23:45:33');

|    d1     |          d2        |          d3        |
| --------- | ------------------ | ------------------ |
| 2017-11-9 | 2017-11-9 23:45:33 | 2017-11-9 23:45:33 |
| 2017-12-9 | 2017-12-9 23:45:33 | 2017-11-9 23:45:33 |