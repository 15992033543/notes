# mysql命令

## 操作数据库

### 显示所有数据库

    SHOW DATABASES;

### 创建数据库

    CREATE DATABASE mydb;

### 创建数据库并指定编码

    CREATE DATABASE mydb DEFAULT CHARACTER SET UTF8;

### 删除数据库

    DROP DATABASE mydb;

### 使用数据库：

    USE mydb;

## 操作表

### 显示数据库中所有表

    SHOW TABLES;

### 创建表

    CREATE TABLE mytable(
      id INT,
      name VARCHAR(4)
    )

字段名在前，类型在后，括号内的数字表示最大长度

### 增加一列

    ALTER TABLE mytable ADD COLUMN age INT;

### 修改已有列

    ALTER TABLE mytable CHANGE name bname VARCHAR(12);

注意：如果表中有数据时，在修改列时可能会失败，比如表中存有一个长度为4的name，此时如果将name的最大长度改为3，则会失败

### 删除指定列

    ALTER TABLE mytable DROP name;

此列的所有数据均被删除

### 重命名表

    ALTER TABLE mytable RENAME TO newtable;

重命名表不会删除表数据

### 删除表

    DROP TABLE mytable;

## 表数据操作

### 插入数据

    # 向指定列插入数据
    INSERT INTO mytable(id,name) VALUES(1,"tom");

    # 如果要向所有列插入数据，可以用下面的方式进行简写，注意用这种方式插入数据，必需要按照列的顺序插入
    INSERT INTO mytable VALUES(2,'jack');

### 查询数据

    # 查询所有列的数据，号表示所有列
    SELECT * FROM mytable;

    # 只查询name与age的数据
    SELECT name,age FROM mytable;

    # 给列指定别名
    SELECT name AS "姓名",age AS "年龄" FROM mytable;

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

    # 删除所有数据，方法一
    DELETE FROM mytable;

    # 方法二
    # 相比方法一，有以下特性：
    # 1.效率更高；
    # 2.可以清除自增长；
    # 3.只能用于删除全部数据，不能带条件删除
    TRUNCATE TABLE mytable;

    # 根据条件删除数据
    DELETE FROM mytable WHERE id=1;

## 查询进阶

### 分页查询

    # 查询表中前5条
    SELECT * FROM mytable LIMIT 5;

    # 从第5条开始（第一条为0），查询10条
    SELECT * FROM mytable LIMIT 5,10;

### 排序查询

    # 升序查询
    SELECT * FROM mytable ORDER BY age ASC;

    # 降序查询
    SELECT * FROM mytable ORDER BY age DESC;

### 模糊查询

    # %表示任意多个字符，_表示一个字符
    SELECT * FROM mytable WHERE name like %tom%;

### null与非null查询

    # 查询name为null的数据
    SELECT * FROM mytable WHERE name IS NULL;

    # 查询name为非null的数据
    SELECT * FROM mytable WHERE name IS NOT NULL;

### 聚合函数查询

    # 查询总记录数
    SELECT COUNT(1) FROM mytable;

    # 查询表中最大的age
    SELECT MAX(age) FROM mytable;

    # 查询表中最小的age
    SELECT MIN(age) FROM mytable;

    # 查询表中age的平均值
    SELECT AVG(age) FROM mytable;

### 分组查询

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

### INT(n)

整型，默认最大存储9位数。注意n并不是指最大长度，而是指配合ZEROFILL使用时进行0填充。

    CREATE TABLE mytable(
        id INT(4) ZEROFILL;
    )

    INSERT INTO mytable(id) VALUES(2);

|  id  |
| ---- |
| 0002 |

### CHAR(n)

字符，默认长度为1，如果指定了n，则长度为n。插入数据时如果不足n位，数据库也会占用n位的空间。

### varchar(n）

字符串，创建表时必须指定n，不然会报错。插入数据时如果不足n位，数据库只会占用插入数据位数的空间。

### ENUM

枚举，表示n选1，插入数据时只能从指定的值中选一个，不然会报错。

    CREATE TABLE mytable(
        country ENUM('English','America','China')
    );

    # Query OK
    INSERT INTO mytable VALUES('China');

    # ERROR
    INSERT INTO mytable VALUES('Japan');

### SET

多选，表示n选n，插入数据时可以从指定值中选一个或者多个插入，用“,”隔开。

    CREATE TABLE mytable(
        country ENUM('English','America','China')
    );

    # Query OK
    INSERT INTO mytable VALUES('China,English');

|    country    |
| ------------- |
| China,English |

### DATE

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

## 表约束

为了防止数据库中存有非法数据，对数据的录入进行限制。

### 非空约束

    # 必须插入，否则会报错
    CREATE TABLE mytable(
        name VARCHAR(10) NOT NULL;
        age INT NOT NULL;
    );

### 默认约束

    # 可以使用DEFAULT关键字指定默认值，不插入时自动插入默认值
    CREATE TABLE mytable(
        name VARCHAR(10);
        age INT DEFAULT 12;
    );

### 唯一约束

    # 使用UNIQUE设置值唯一，不能插入相同的值，否则报错
    CREATE TABLE mytable(
        id varchar(10) UNIQUE
    );

### 主键约束

主键约束 = 唯一约束 + 非空约束，通常在一个表中，只有一个主键，常用作id等字段。

    CREATE TABLE mytable(
        id INT PRIMARY KEY
    );

    # 配合AUTO_INCREMENT实现自增长，插入数据时会自动插入，且唯一
    CREATE TABLE mytable(
        id INT PRIMARY KEY AUTO_INCREMENT
    );

### 复合主键

有时候一个字段不能确定主键，需要多个字段才能确定主键，可以使用复合主键。

    # name与dept作为复合主键
    CREATE TABLE mytable(
        name VARCHAR(10),
        dept VARCHAR(10),
        PRIMARY KEY(name,dept)
    );

    # Query OK
    INSERT INTO mytable VALUES('tom','kaifa');
    # Query OK
    INSERT INTO mytable VALUES('tom','renshi');
    # ERROR
    INSERT INTO mytable VALUES('tom','renshi');

### 外键约束（慎用）

外键约束可以使两个表建立约束，防止一些无意义的数据。

下面这个表dep字段出现了大量的冗余：

| id  |   name  |  dep   |
| --- | ------- | ------- |
|  1  |    tom  | 开发部 |
|  2  |   jack  | 开发部 |
|  3  |   tom   | 开发部 |
|  4  |   张三   | 测试部 |
|  5  |  李四    | 测试部 |
|  6  |  王五    | 测试部 |

可以拆分为两个表，一个为emp表，一个为dep表，可以通过emp表的did找到dep表中对应的部门，以减少冗余。

emp:

| id  |   name  |  did   |
| --- | ------- | ------- |
|  1  |    tom  |    1   |
|  2  |   jack  |   1   |
|  3  |   tom   |   1    |
|  4  |   张三   |   2   |
|  5  |  李四    |   2   |
|  6  |  王五    |  2   |

dep:

| id  |   dname  |
| --- | ------- |
|  1  |  开发部  |
|  2  |  测试部  |

但是这里会有一个问题，比如执行下面语句：

    INSERT INTO emp(name,did) VALUES('rose',3);

因为dep表中并没有id为3的部门，所以这条数据的存在是没有意义的。

这时外键约束就派上用场了：

    # 创建dep表
    CREATE TABLE dep(
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(10) NOT NULL
    );

    # 创建emp表，并添加外键约束，让did与dep表的id关联
    CREATE TABLE emp(
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(10) NOT NULL,
        did INT NOT NULL,
        FOREIGN KEY (did) REFERENCES dep(id)
    );

    INSERT INTO dep(dname) VALUES('开发部');

    # Query OK
    INSERT INTO emp(name,did) VALUES('tom',1);

    # ERROR，因为dep表中并没有id为2的部门
    INSERT INTO emp(name,did) VALUES('tom',2);

如果创建表时忘记添加外键约束，可以用以下方法添加：

    ALTER TABLE emp ADD CONSTRAINT fk_emp_dep FOREIGN KEY (did) REFERENCES dep(id);

设置了外键约束后，删除dep中的数据时，会报错：

    # ERROR
    DELETE FROM dep WHERE id=1;

因为dep中的数据被emp引用了，所以必须删除emp中引用了dep的数据：

    DELETE FROM emp WHERE id=1;

    # Query OK
    DELETE FROM dep WHERE id=1;

也可以在设置外键约束时启用级联删除，这样在删除数据时，会一并删除引用了此数据的数据：

    CREATE TABLE emp(
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(10) NOT NULL,
        did INT NOT NULL,
        FOREIGN KEY (did) REFERENCES dep(id) ON DELETE CASCADE
    );

同理，可启用级联更新：

    CREATE TABLE emp(
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(10) NOT NULL,
        did INT NOT NULL,
        FOREIGN KEY (did) REFERENCES dep(id) ON DELETE CASCADE ON UPDATE CASCADE
    );

注意！慎用级联删除与级联更新时，因为在操作一张表的同时影响了另一张表，可能会造成不可挽回的损失！

## 触发器

触发器的作用是操作一张表的时候，同时做另外一些事情，相当于事件。

当t1表新增数据后，指定t2表要做的操作：

    CREATE TRIGGER my_trigger AFTER INSERT ON t1 FOR EACH ROW INSERT INTO t2(name) VALUES('jack');

当t1表更新数据后，指定t2表要做的操作：

    CREATE TRIGGER my_trigger AFTER UPDATE ON t1 FOR EACH ROW UPDATE t2 SET name="rose" WHERE name="jack";

当t1表删除数据后，指定t2表要做的操作：

    CREATE TRIGGER my_trigger AFTER DELETE ON t1 FOR EACH ROW DELETE FROM t2 WHERE name="jack";

删除触发器：

    DROP TRIGGER my_trigger;

## 多表查询

有以下两张表

emp:

| id  |   name  |  did   |
| --- | ------- | ------- |
|  1  |    tom  |    1   |
|  2  |   jack  |   1   |
|  3  |   tom   |   1    |
|  4  |   张三   |   2   |
|  5  |  李四    |   2   |
|  6  |  王五    |  2   |

dep:

| id  |   name  |
| --- | ------- |
|  1  |  开发部  |
|  2  |  测试部  |

### 子查询

使用子查询查询出部门为开发部的所有员工的姓名：

    SELECT name FROM emp WHERE did=(SELECT id FROM dep WHERE name="开发部");

### 关联查询

使用内连接查询出部门为开发部的所有员工信息：

    # 方法一
    SELECT e.name FROM emp e,dep d WHERE e.did=d.id AND d.name="开发部";

    # 方法二，使用INNER JOIN进行表关联
    SELECT e.name FROM emp e INNER JOIN dep d ON e.did=d.id AND d.name="开发部";

使用左外连接查询出部门为开发部的所有员工信息：

    # 用dep(称为主表或左表)去匹配epm(从表)
    SELECT e.name FROM dep d LEFT JOIN emp e ON e.did=d.id AND d.name="开发部";

使用右外连接查询出部门为开发部的所有员工信息：

    SELECT e.name FROM emp e RIGHT JOIN dep d ON e.did=d.id AND d.name="开发部";