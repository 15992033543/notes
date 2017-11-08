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

    // 只查询name与age的数据
    SELECT name,age FROM mytable;

    // 给列指定别名
    SELECT name AS "姓名",age AS "年龄" FROM mytable;

按条件查询：

    // 单条件查询
    SELECT * FROM mytable WHERE id=1;

    // AND表示并且
    SELECT * FROM mytable WHERE age=12 AND name="tom";

    // OR表示或者
    SELECT * FROM mytable WHERE age=12 OR name="tom";

    // IN表示一个区间，age满足括号内其中一个即可
    SELECT * FROM mytable WHERE age IN (12,15,20);

    // age小于15
    SELECT * FROM mytable WHERE age<15;

    // age介于10到20之间
    SELECT * FROM mytable WHERE age BETWEEN 10 AND 20;

### 修改数据

    // 将tom的age改为13
    UPDATE mytable SET age=13 WHERE name="tom";

    // 同时修改多个列
    UPDATE mytable SET age=13,gender="male" WHERE name="tom";

### 删除数据

删除所有数据：

    // 第一种方法
    DELETE FROM mytable;

    /**
     * 此方法也可以删除表中所有数据，相对于第一种方法，有三个特性：
     * 1.效率更高；
     * 2.可以清除自增长；
     * 3.只能用于删除全部数据，不能带条件删除
     */
    TRUNCATE TABLE mytable;

根据条件删除数据：

    // 删除id为1的数据
    DELETE FROM mytable WHERE id=1;