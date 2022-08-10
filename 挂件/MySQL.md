# MySOL数据库

数据库是用来组织、存储和管理数据的仓库

- MySQL属于传统型数据库

数据的组织结构：指的就是数据以什么样的结构进行存储

**传统型数据库的数据组织结构**

在传统型数据库中，数据的组织结构分为数据库(database)、数据表(table)、数据行(row)、字段(field)这 4 大部分组成

实际开发中库、表、行、字段的关系：

- 在实际项目开发中，一般情况下，每个项目都对应独立的数据库。 
- 不同的数据，要存储到数据库的不同表中，比如用户数据存储到 users 表中，图书数据存储到 books 表中
- 每个表中具体存储哪些信息，由字段来决定，比如为 users 表设计 id、username、password 这 3 个字段
- 表中的行，代表每一条具体的数据

**创建数据表**

DataType 数据类型：① int 整数;② varchar(len) 字符串;③ tinyint(1) 布尔值

字段的特殊标识：① PK：主键、唯一标识;② NN：值不允许为空;③ UQ：值唯一;④ AI：值自增

**使用 SQL 管理数据库**

SQL结构化查询语言，专门用来操作数据库的编程语言

1. SQL 是一门数据库编程语言
2. 使用 SQL 语言编写出来的代码，叫做 SQL 语句
3. SQL 语言只能在关系型数据库中使用（例如 MySQL、Oracle、SQL Server）。非关系型数据库（例如 Mongodb）不支持 SQL 语言

## MySQL的增删改查

- --是sql语句中的注释

- 查询-navicat查询：

```
select * from 表名
```

- 增加-navicat查询：

```
insert into c1(name) values('武夜零')
```

- 修改-navicat查询：

```
update c1 set name='武夜伽' where name='武夜零'
```

- 删除-navicat查询：

```
delete from c1 where name='武夜伽'
```

注：SQL 语句的关键字大小写不敏感

## 增删改查辅助性语句

**where条件选择语句**

where语句用于限定选择的标准。适用于删改查

where的运算符：

![](E:\HTML5\图片文件夹\图片1.png)

注：在某些版本的 SQL 中，操作符 <> 可以写为 !=

- 条件查询-navicat查询：

```
select * from c1 where age=17
```

- SQL的and和or运算符:把多个条件结合起来,and相当于&&,or相当于||

**order by排序语句**

order by语句用于排序

order by语句默认升序

注：从小到大称为升序，从大到小称为降序

- 升序：

```
select * from c1 order by age//asc
```

- 降序：2

```
select * from c1 order by age desc
```

**count(*) 函数**

返回查询结果的总数据条数

- 返回数据条数：

```
select count(*) from c1
```

- 设置别名：

```
select count(*) as kingqueen from c1 
```

## node.js项目中操作MySQL

mysql 模块是托管于 npm 上的第三方模块。它提供了在 Node.js 项目中连接和操作 MySQL 数据库的能力

- 安装MySQL模块-js:

```
npm i mysql
```

- 连接到 MySQL 数据库-js:

```
const mysql = require('mysql');
//建立与 MySQL 数据库的连接关系
const queen = mysql.createPool({
    host: '127.0.0.1', // 数据库的 IP 地址
    user: 'root', // 登录数据库的账号
    password: '135789', // 登录数据库的密码
    database: 'crazy', // 指定要操作哪个数据库
})
```

- 查询-js:

```
queen.query('select * from c1', (err, results) => {
    if (err) return console.log(err.message)
    console.log(results)
})
```

- 增加-js:

```
const n9m9 = { name: '虎符太一' }
const acc = 'insert into c1 (name) values (?)'
queen.query(acc, n9m9.name, (err, results) => {
    if (err) return console.log(err.message)
})
```

注:?是占位符

- 修改-js:

```
const n9m9 = { name: '少大司命' }
const acc = 'update c1 set name=? where name=?'
queen.query(acc, [n9m9.name, '虎符太一'], (err, results) => {
    if (err) return console.log(err.message)
})
```

- 删除-js:

```
const acc = 'delete from c1 where name=?'
queen.query(acc, '少大司命', (err, results) => {
    if (err) return console.log(err.message)
})
```

- 标记删除-js:

```
const acc = 'update c1 set age=? where name=?'
queen.query(acc, [1, '虎符太一'], (err, results) => {
    if (err) return console.log(err.message)
})
```

注:标记删除的本质是将某个数据改为1，实质只是修改而已，根本就不是删除