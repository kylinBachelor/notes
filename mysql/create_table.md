## 创建表

#### 表创建基础
1. 为利用CREATE TABLE 创建表，必须给出系列定义:
	- 新表的名字, 在关键字CREATE TABLE 之后给出
	- 表列的名字和定义，用逗号分隔
2. CREATE TABLE语句也可能会包括其他关键字或选项，但至少要包括表的名字和列的细节
```sql
# if not exists 判断表是否存在，不存在才创建表
CREATE TABLE if not exists customers ( 
	cust_id     	int         NOT NULL AUTO_INCREMENT,
	cust_name   	char(50)    NOT NULL DEFAULT JAVA_NAME,
	cust_address	char(50)	NULL,
	cust_email		char(255)	NULL,
	PRIMARY KEY (cust_id,cust_name) -- 主键
) ENGINE=InnoDB;
```

#### 表更新(添加列)
```sql
ALTER TABLE customers ADD cust_phone CHAR(20);
```

#### 表更新(删除列)
```sql
ALTER TABLE customers DROP COLUMN cust_phone
```

#### 表更新(定义外键)
```sql
ALTER TABLE customers ADD CONSTRAINT cust_foreignkey

```

#### 删除表
```sql
DROP TABLE customers
```

#### 重命名表
```sql
RENAME TABLE customer TO customers  -- customers重命名为customers
```

## 使用视图
#### 视图常见的应用
* 重用SQL语句。
* 简化复杂的SQL操作，在编写查询后，可以方便的重用它而不必知道它的基本查询细节。
* 使用表的组成部分，而不是整个表。
* 保护数据，可以给用户授予表的特定部分的访问权限而不是整个表的访问权限。
* 更改数据格式和表示。视图可返回与底层表的表示和格式不同的数据。

#### 视图的规则和限制
* 与表一样试图必须唯一命名（不能给视图取与别的视图或表相同的名字）。
* 对于可以创建的视图数目没有限制。
* 为了创建视图必须具有足够的访问权限。这些限制通常由数据库管理人员授予
* 视图可以嵌套，即可以利用从其他视图中检索数据的查询来构造一个视图
* ORDER BY 可以用在视图中，但是如果从该视图检索数据SELECT中也含有ORDER BY,那么该视图中的ORDER BY将被覆盖。
* 视图不能索引，也不能有关联的触发器或默认值。
* 视图可以和表一起使用。例如，编写一条联结表和视图的SELECT语句。

#### 使用视图
* 视图用 CREATE VIEW viewname as sql查询语句 语句来创建。
* 使用SHOW CREATE VIEW viewname; 来查看创建视图的语句。
* 使用DROP删除视图，其语法为DROP VIEW viewname。
* 更新视图时可以先用DROP再用CREATE,也可以直接用CREATE OR REPLACE VIEW。如果要更新的视图不存在，则第二条更新语句会创建一个视图；如果要更新的视图存在，则第二条更新语句会替换原有视图。

## 存储过程
