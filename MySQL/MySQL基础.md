

##  1  mysql 安装及启动

服务启动：

- 通过电脑服务选择启动
- 命令行启动<font color="red">(注意权限一般得管理员权限)</font>：net start mysql  服务名
- 命令行停止：net stop mysql  服务名

登入：

  mysql -h 主机名 -P 端口号 -u 用户名 -p 密码  

```
mysql -u root -p
```

## 2 mysql 基本操作

```mysql
drop database if EXISTS dbtest; #删除数据库
# 创建数据库 dbtest
create database dbtest ;
# 使用数据局dbtest
use dbtest;
# 创建表格
create table tab_test(
	id int primary key auto_increment,
	username varchar(32) not null unique,
	password varchar(32) not null,
	email   varchar(20) 
);
# 查看表结构
desc tab_test;
# 插入记录
insert into tab_test(id,username,password,email) values(1,"lancer","123456","lancer@163.com" );
insert into tab_test(id,username,password,email) values(2,"xiaoming","123456","xiaoming@163.com" );
# 查询记录
select * from tab_test;
# 修改记录
update tab_test set email="lancerupdate@163.com" where username="lancer" ;
# 删除记录
delete from tab_test where username="lancer" ;
select * from tab_test;
```

结果如下：

<img src="img\mysql基础操作.png" style="zoom:80%;" />

## 3 mysql数据类型



## 4 SQL语言规范及 SQL分类

### 1 SQL的语言规范

- mysql 对于SQL语句不区分大小写，SQL语句关键字尽量大写。



### 2 SQL分类

- DDL(Data Definition Language): 数据定义语言，这些语句定义了不同的数据段，数据库，表，列，索引等数据库对象。常见关键字包括create，drop,alter等。
- DML(Data Munipulation Language): 数据操纵语言，用于添加，删除，更新和查询数据库记录，常见关键字包括：insert, delete,update和select 等。
- DCL(Data Control Language): 数据控制语句。用于控制不同数据段直接的许可和访问级别的语句，主要关键字：grant ,revoke等。



## 5  DDL

### 1  操作Database

```mysql

/*  1 创建数据库  
    create database 数据库名 [charset 字符集 ]  
    如果不指定字符集,则按安装mysql服务时选择的默认字符集。
  */
create database  dbname ;

/*  2 查看有哪些数据库 
    show databases;
    当前用户有权限看的
*/
show databases;

/* 3 选择数据库
    use 数据库名 ;
*/
use dbname ;

/* 4 查看当前正在使用哪个数据库
    select database();
 */
select database();

/* 5 删除数据库
    drop database 数据库名; 
 */
 drop database dbname ;
 
```

![](img\DDL.png)

###  2 表结构操作

```mysql
/* 选择数据库 */
use dbtest ;

drop table  if exists book ;

/*  创建表：
    create table 表明(
        列名 列的类型 [ (长度) 约束 ], 
        列名 列的类型 [ (长度) 约束 ], 
        列名 列的类型 [ (长度) 约束 ]
    );
 */
create table book(
    id  int, #编号
    bookname varchar(20) not null,  #书名
    price    double, #价格 
    author   int , #作者编号
    publishData datetime
);

desc book ;

/*  标的修改: 1 修改列名 2 修改列的类型或约束 3 增加列
    4 删除列  5 修改表名 
 */

/*
    1 修改列名:
    alter table tablename change [column] old_name  column_definition; 
*/
alter table book change column publishData pubDate datetime ;
desc book ;
/*
    2 修改列的类型或者约束
    alter table tablename modify [column] 列名 column_definition
*/
alter table book modify column pubDate TIMESTAMP ;
desc book ;
/*
    3 增加新列
    alter table tablename add [column] column_definition [FIRST|AFTER col_name ] ; 
*/
alter table book add column annual double ;
desc book ;
/*
    4 删除列
    alter table tablename drop [column] col_name ; 
*/
alter table book drop column annual ; 
desc book ;

/*
    5 修改表名
    alter table tablename rename [to] new_tablename ;
*/
alter table book rename to book_new ; 

DESC book_new ;

/*
    删除表：
    drop table table_name ;
*/
drop table if exists book_new ; 

/* 选择数据库 下图未显示该段代码结果 */
use dbtest ;
drop table if exists author ;
create table author(
    id   bigint ,
    name varchar(20) ,
    nation varchar(20)
);
insert into author values(1,"lancer","China");

drop table if exists author1 ;
drop table if exists author2 ;
#只复制结构
create table author2 like author ;
select * from author2 ;
DESC author2 ;

#表的复制(结构数据)
create  table author1 select * from author ;
select * from author1 ;
```
<div>
	<img src="img\table_op.png">
</div>

## 6 DML

DML操作是对数据库中表记录的操作，包括插入（insert）,更新（update）,删除（delete）和查询（select）。

###  查询记录

------
基础查询：
```mysql
### 先建立好了数据库myemployees 数据库中有表employees
use myemployees;
show tables; 
DESC employees; 
/*
    查询记录(各式各样查询 只列出最简单的格式):
    select * from tablename [ where condition]
    * 选所有记录,也可以用逗号分割所有字段来代替(可控制顺序)
*/

/*  一  基础查询   */
# 1 查询表中单个字段
select first_name from employees ;
# 2 查询表中的多个字段
select first_name, last_name from employees ;
# 3 查询表中的所有字段
select * from employees; 
#4 查询常量
select 100 ; 
#5 查询表达式
select 100%98; 
#6 查询函数
select VERSION();

# 7 起别名(便于理解 区分重名)
#使用as 
select 100%89 as result; 
#使用空格
select last_name  姓, first_name  名 from employees ;
# 显示结果为out put 
select salary as "out put" from employees ;
#8 去重
select distinct salary from employees ;
## 拼接函数
select concat(last_name,first_name) as 姓名 from employees ; 

/* 二 条件查询  */
/*
分类:
    1 按条件表达式筛选
        条件运算符: > < = != <> >= <= 
    2 按逻辑表达式筛选
        逻辑运算符:
          && ||  !
          and or not 
    3 模糊查询
        like 
        between and 
        in 
        is null 
*/

#  1 按条件表达式筛选
select * from employees where salary >12000 ;
select last_name,department_id from employees where department_id <> 80 ;

# 2 按逻辑表达式筛选
select last_name,salary from employees where salary>=10000 and salary<= 18000 ;


/*  3 模糊查询
    like 
    特点:
    1 一般和通配符搭配使用
        通配符:
        % 任意多个字符,包含0
        _ 任意单个字符

*/
## 案例1  查询员工名 中包含'a'的员工信息(默认不区分大小写)
select * from employees where last_name like '%a%';
## 案例2  查询员工名中第三个字符为n 第五个字符为l的员工名和工资
select last_name,salary from  employees where last_name like '__n_l%';
## 案例3  查询员工名中第二个字符为 _ 的员工名
select  last_name from employees where last_name like '_\_%';

## 查询员工的工种编号是 'IT_PORT' ,'AD_VP','AD_PRES'
/*
in 判断某字段的值是否属于in列表的某一项
in 列表的值类型必须统一或者兼容
*/
select last_name,job_id from employees where job_id in ('IT_PORT' ,'AD_VP','AD_PRES');

/*
    =或者<>不能用于判断null值
    IS NULL: 仅仅可以判断NULL值,可读性高,建议使用
    <=>  :既可以判断NULL值,也可以判断普通值。

*/
# 查询没有奖金的员工名和奖金率
select last_name,commission_pct from employees where  commission_pct is null ;
select last_name,commission_pct from employees where  commission_pct is not null ;
```

### 插入记录



### 更新记录



### 删除记录













































