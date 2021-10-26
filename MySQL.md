<!--
 * @Author: FANG
 * @Date: 2021-10-21 00:37:53
-->
# MySQL

### SQL分类

1. DDL(*Data Definition Language*) 数据定义语言

   用来定义数据库对象：数据库 表 列等。关键字 create drop alter 等

2. DML(*Data Manipulation Language*) 数据操作语言 用来对数据库进行增删改。

   关键字insert delete update 等

3. DQL(*Data Query Language*) 数据查询语言

   用来查询数据库表中的记录(数据)。关键字 select where 等

4. DCL(*Data control Language*) 数据控制语言 (了解)

   用来定义数据库访问权限和安全级别，及创建用户。

   关键字：GRANT REVOKE 等

### SQL
#### DDL
1. 操作数据库
   1. *show databases; 显示所有数据库名称*
   2. *show create database 数据库名; 查询创建此数据库的语法* / 查询数据库的字符集
   3. *create database 名称; 创建数据库* (create database if not exists 名称;) 先查询存不存在再创建*
   *create database 名称 character set gbk; (create database 名称 if not exists set character gbk;)*
   1. *alter database 名称 character set 字符集名称;*
   2. drop database 名称;
   3. select database(); 查询正在使用的数据库名称
   4. 使用数据库 use 名称;
2. 操作表
   1. show tables; 查询所有表的名称
   2. desc 表名; 查询表结构
   3. create table 表名(
         列名1 数据类型1,
         列名2 数据类型2,
         ....
         列名n 数据类型n
   );
   4. drop table 名称; 删除表
   5. create table 名称 like 被复制表名称; 复制表
   6. alter table 名称 rename to 名称; 修改表的名称
   7. alter table 名称 set character 字符集; 修改表的字符集
   8. alter table 名称 add 列名 数据类型;
   9. alter table 名称 change 列名 新列名 新数据类型;
      alter table 名称 modify 列名 新数据类型;
   10. alter table 名称 drop 列名;

#### DML 增删改表中数据
1. insert into 表名(列名1, 列名2, .... 列名n) values(值1, 值2, .... 值n);
   inset into 表名 values(值1, 值2 .... 值n) 默认给列所有赋值
2. delete from 表名 [where 表名];  delete from 表名; 全删
   truncate table 表名; 删除表然后再创建一个一样的空表;
3. update 表名 set 列名1 = 值1, .... [where 条件];

#### DQL 查询语句
##### 单表查询
1. 基础查询 (略)
2. 条件查询 (略)
3. 条件查询_模糊查询 (略)
4. 排序查询 (略)
5. 聚合函数(纵向计算)
   1. count:   
   2. max:
   3. min:
   4. sum:
   5. avg: average
    * 聚合函数除了count(*)均跳过了null值
6. 分组查询 (略)
7. 分页查询
      limit 开始索引, 每页查询的条数
      select * from student limit 0, 3;--第一页三条记录
      select * from student limit 3, 3;--第二页三条记录
      公式: 开始的索引 = (当前页码 - 1) * 每页显示的条数 

##### 多表查询
1. 隐式内连接: select * from emp, dept where emp.dept_id == dept.id;
2. 显示内连接: select * from emp [inner] join dept on emp.dept_id == dept.id;
3. 左外连接: select 字段列表 from 表1 left [outer] join 表2 on 条件
      查询的是左表所有数据以及其交集部分
4. 右外连接: select 字段列表 from 表1 right [outer] join 表2 on 条件
      查询的是左表所有数据以及其交集部分
5. 子查询: select * from emp where emp.salary = (select max(salary) from emp);
   1. 子查询的结果是单行单列：子查询可以作为条件，使用运算符去判断
   2. 子查询的结果是多行单列：子查询的结果可以作为条件，使用in来判断
   3. 子查询的结果是多行多列：子查询可以作为一张虚拟表

#### 约束
概念: 对表中的数据进行限定 保证数据的正确性 有效性 完整性
* 分类：
   1. 主键约束：primary key
   2. 非空约束：not null
   3. 唯一约束：unique
   4. 外键约束：foreign key
* 非空约束：not null
     创建表时添加约束
      create table stu(
         id int,
         name varchar(20) not null -- name非空
      );
      alter table stu modify name varchar(20);
      alter table stu modify name varchar(20) not null;
* 唯一约束：unique
      create table stu(
         id int,
         phone_number varchar(20) unique -- phone_number唯一
      );
      null 值可以重复
      alter table stu drop index phone_number;
      alter table stu modify phone_number varchar(20) unique;
* 主键约束：primary key 非空且唯一 一张表只能有一个字段为主键
      create table stu(
         id int primary key
      );
      alter table stu drop primary key;
      alter table stu modify id int primary key;
      主键自增长：auto_increment
      create table stu(
         id int primary key auto_increment
      );
      alter table stu modify id int;
      alter talbe stu modify id int auto_increment;
* 外键约束：foreign key
      create table stu (
         ....,
         constraint 外键名称 foreign key 外键列名称 references 主表名称(主表列名称)
      );
      alter table stu drop foreign key 外键名;
      alter table stu add constraint 外键名称 foreign key 外键列名称 references 主表名称(主表列名称);
      create table stu (
         ....,
         constraint 外键名称 foreign key 外键列名称 references 主表名称(主表列名称) on update cascade;
      );
      设置级联更新
      on delete cascade 级联删除
