

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

### 3 常见约束





## 6 DML

DML操作是对数据库中表记录的操作，包括插入（insert）,更新（update）,删除（delete）和查询（select）。

###  查询记录

------
####  基础查询：

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

#### 排序查询：

```mysql

/*  排序查询  
    select 查询列表 from 表 [where 条件] 
    order by 排序列表 [ASC| DESC]

    1  desc 代表降序  asc代表升序  不写默认升序 
    2  order by 句子中支持单个字段,多个字段,表达式，函数,别名
    3  order by 子句一般是放在查询语句最后面, limit子句除外。
 */

/* 案例1   查询员工信息 要求工作从高到低   */
select * from employees order by salary DESC;
select * from employees order by salary;

/*-  案例2 查询部门编号>=90的员工信息 按照入职时间先后进行排序（添加筛选条件）  */
select * from employees where department_id >= 90 order by hiredate ASC ;

/* 案例3: 按照年薪高低显示员工信息和年薪(按表达式排序) */
select *, salary*12*(1+IFNULL(commission_pct,0)) from employees 
order by salary*12*(1+IFNULL(commission_pct,0))  DESC ;

/* 案例4: 按照年薪高低显示员工信息和年薪(按别名式排序) */
select *, salary*12*(1+IFNULL(commission_pct,0)) AS 年薪
from employees 
order by 年薪  DESC ;

/* 案例5  按姓名的长度显示员工的姓名和工资 (按函数排序)   */
select LENGTH(last_name) 字节长度 ,last_name ,salary 
from employees 
order by  LENGTH(last_name) DESC ;

/*-  案例6  查询员工信息  要求先按工资升序 再按员工编号降序（按多个字段排序）  */
select * from  employees  
order by salary ASC , employee_id DESC ;
```

分组查询：

```mysql
/*-  分组函数
分类:
    sum求和，avg平均值, max最大值, min最小值，count计算个数

  */
# 简单使用
select sum(salary) from employees;
select avg(salary) from employees;
select max(salary) from employees;
select min(salary) from employees;
select count(salary) from employees;

select sum(salary) 求和,  max(salary) 最大值  from employees;

/*- 分组查询 (初步  待细学习) */

/*- 查询每个部门的平均工资  */
select  avg(salary)  from employees ;

/*- 查询每个工种的最高工资  */
select  max(salary) , job_id from employees
group by job_id ;

/*-  查询每个位置的部门个数  */
select  count(*), location_id from departments
group by location_id ;

/*-  查询邮箱包含字母'a' 每个部门的平均工资  */
select avg(salary) ,department_id from  employees
where email like '%a%' group by department_id ;
```

#### 连接查询：

```mysql
/*- 连接查询
    含义:又称多表查询, 当查询的字段来自多个表时,就会用到连接查询。
    笛卡尔乘积现象：
        表1  有m行 表2 有n行 结果=m*n行
        
分类:
    按年代:
    sql92标准: 仅仅支持内连接
    sql99标准(推荐)：支持内连接+外连接(左外和右外)+交叉连接


    按功能：
        内连接:
            等值连接
            非等值连接
            自连接
        外连接：
            左外连接
            右外连接
            全外连接
        交叉连接:
  */

-- 一 sql92标准

-- 1 等值连接
select name , boyName from boys,beauty 
where beauty.boyfriend_id = boys.id ;
/*- 案例2 查询员工名和对应的部门名  */
select last_name, department_name from  employees, departments
where employees.department_id = departments.department_id;
-- 为表起别名
/*-  查询员工名,工种号, 工种号 */
select last_name, e.job_id, job_title
from  employees e, jobs j
where e.job_id = j.job_id;

-- 2 非等值连接
/*- 案例1: 查询员工的工资和工资  */

-- 3 自连接
/*- 查询 员工名和上级名称  */
select e.employee_id, e.last_name, m.employee_id,m.last_name
from employees e ,employees m 
where e.manager_id = m.employee_id ;

/*- 
    二  sql99语法
    select 查询列表
    from 表1 别名 [连接类型]
    join 表2 别名 [连接类型]
    on 连接条件
    [where 筛选条件]
    [group by  分组]
    [having 筛选条件]
    [order by 排序列表]
分类：
    内连接: inner
    外连接：
        左外：left [outer]
        右外：right[outer]
        全外：full [outer]
    交叉连接: cross 

  */

  # 等值连接
  # 案例1  查询员工名,部门名
  select last_name, department_name from employees e 
  inner join departments d 
  on e.department_id = d.department_id ;
```

#### 子查询：

```
## 待添加
```

#### 分页查询

```mysql

/*-  
分页查询:
应用场景:当要显示数据,一页显示不全,需要分页提交sql请求

    limit [offset] size; 
    offset 起始索引(0开始)
    size   要显示的条目个数
 */

-- 案例1  查询前5条员工信息
select * from employees limit 0, 5 ; 
select * from employees limit  5 ; 
-- 案例2  查询第11条到25条
select * from employees limit 10, 15 ; 
```

#### 联合查询



### 插入记录



### 更新记录



### 删除记录



## 7 常见函数

单行函数：

```mysql

/*
    分类:
    1 单行函数
        如 concat,length, ifnull等
    2 分组函数
        功能: 做统计时使用，又称统计函数，聚合函数，组函数。
   */

/*
    字符函数
    数学函数
    日期函数
    其他函数
    流程控制函数
*/

/*- 一 字符函数   */

# 1 length 获取参数值得字节个数
select length("Tom") ;
select length("小明ming");
# 2. concat 拼接字符串
select concat(last_name,'_',first_name) 姓名 from employees ;
# 3. upper, lower
select upper('john');
select lower('jOHn');
select concat(upper(last_name),lower(first_name)) 姓名
from employees ;
#4 substr,substring  注意索引从1开始
#截取指定索引后面字符
select  substr("好好学习天天向上",7) output ;
# 截取指定字符长度字符
select  substr("好好学习天天向上",3,2) output ;
# 5  instr 返回子串第一次出现的索引 如果找不到返回0
select instr("TOMHH","H") as output ;
#6 trim  
select length(trim(" hhhh   ")) as output ;
select trim('a'from 'aaaaJJJJJJJJJJJaaaaa')  as output ;

/*-  二 数学函数  */
# round 四舍五入
select  round(3.6) ;
select  round(-3.6) ;
select  round(3.634,2) ;
# ceil 返回大于该参数的最小整数 向上取整
select ceil(1.02);
# floor 向下取整  返回小于该数的最大整数值
select floor(1.02);
# truncate 截断
select truncate(3.155678, 3);
# mod 取余
select mod(10,3);

/*- 三 日期函数  */
# new 返回当前系统日期+时间
select now();
# curdate 返回当前系统日期 不包括时间
select curdate();
# curtime 返回当前系统时间 不包括日期
select curtime();
# 获取指定的部分
select year(now()) ;
select year('1999-01-01');
select YEAR(hiredate) 年 from  employees;
select MONTH(now()); #月
#  str_to_date  将字符通过指定格式转化为日期
select str_to_date('1990-3-9', '%Y-%c-%d') as output; 
# date_format  将日期转化为字符
select  date_format('2020/10/01','%Y年%m月%d日');
select  date_format(now(), '%Y年%m月%d日');

/*-  四 其他函数 */
select version();
select database();
select user();

/*-  五 流程控制函数  */
# if 
select if(10>5,"大","小");
# ifnull等
```











































