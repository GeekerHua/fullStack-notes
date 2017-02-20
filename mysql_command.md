## 数据库
了解SQL和MySQL。SQL语句的关键词推荐使用大写进行区分，良好的习惯对今后会有很大帮助。

### E-R模型

> 当前物理的数据库都是按照E-R模型进行设计的
>
> * E表示entity，实体
> * R表示relationship，关系
>   > 一个实体转换为数据库中的一个表  
>   > 关系描述两个实体之间的对应规则，包括
>   >
>   > * 一对一
>   > * 一对多
>   > * 多对多

关系转换为数据库表中的一个列

* 在关系型数据库中一行就是一个对象

### 三范式

* 第一范式（1NF\)：列不可拆分
* 第二范式（2NF\)：唯一标识
* 第三范式（3NF\)：引用主键

## 数据库命令

| 功能 | 命令 |
| --- | --- |
| 查看所有数据库 | show databases; |
| 查看当前使用的数据库 | select database\(\); |
| 切换数据库 | use `DBname`; |
| 创建数据库 | create database `DBname` charset=utf8; |
| 删除数据库 | DROP DATABASE `DBname`; |

## 数据表命令

| 功能 | 命令 |
| --- | --- |
| 查看所有表 | show tables; |
| 创建表 | create table `Tname` \(id int auto\_increment primary key not null, `…………`\); |
| 删除表 | drop talbe `Tname`; |
| 修改表名 | RENAME TABLE `原名` TO `新名字`; |
| 修改表名 | ALTER TABLE `原名` RENAME `新名字`; |
| 修改表名 | ALTER TABLE `原名` RENAME TO `新名字`; |

### 对一列进行修改(修改表结构)
| 功能 | 命令 | 
--- | ---
增加一列 | ALTER TABLE `表名字` ADD COLUMN `列名字` `数据类型` `约束`;或： ALTER TABLE `表名字` ADD `列名字` `数据类型` `约束`;
删除一列 | ALTER TABLE `表名字` ADD COLUMN `列名字` `数据类型` `约束` 或： ALTER TABLE `表名字` ADD `列名字` `数据类型` `约束`;
修改数据类型 | ALTER TABLE `表名字` MODIFY `列名字` `新数据类型`;
重命名一列 | ALTER TABLE `表名字` CHANGE `原列名` `新列名` `数据类型` `约束`;

#### alter的change与modify命令
** 修改数据类型可能会导致数据丢失，请谨慎考虑**
* change: 修改字段名
* modify: 不修改字段名，修改属性
  > alter table students change name sname varchar\(10\) not null;  
  > alter table students modify name carchar\(10\) not null;

## 数据命令操作crud:

| 功能 | 命令 |
| --- | --- |
| 查询 | select \* from `Tname`; |
| 增加 | insert into `Tname` values\(……\); |
| 修改 | update `Tname` set `key`=`value`; |
| 删除 | delete from `Tname` where `……`; |
| 逻辑删除: 本质就是修改 | alter table `Tname` add isDelete bit default 0; |
| -- | update `Tname` isDelete=1 where `……`; |

## 查询语句

* 最基本的
  * select \* from `Tname` \(as `别名`\);
* 指定字段
  * select \(`field`,`field`\) from `Tname`;
* 消除重复行
  * select distinct `field` from `Tname`;
* where指定运算
  > 对行进筛选,逐行判断
* 比较运算
  * `>, <, !=, =, >=, <=`
  * `select * from`Tname`where id<=4`;
* 逻辑运算
  * and, or ,not
* 模糊查询\(like\)
  > select \* from `Tname` where `field` like `%黄_`;
  
  
  匹配符 | 匹配内容 
  ---- | ---- 
  符号`%`  | 匹配0到多个任意字符 
  符号`_`  | 匹配一个任意字符 

* 范围查询

  > in 表示在一个非连续的范围内  
  > between…and…在一个连续的范围内\(between紧挨着的and是它的匹配语句\)
  >
  > ```sql
  > select * from `Tname` where id in(1,3,8); 
  > select * from `Tname` where id between 2 and 8;
  > ```

* 空判断

  > null与''是不同的。is null来判断是否为空

## 聚合

常用的5个聚合函数

| 函数 | 功能 | 示例 |
| --- | --- | --- |
| count\(\*\) | 计算总行数 | select count\(\*\) from `Tname` where isDelete=0; |
| max\(`field`\) | 求此列最大值 | select max\(id\) from `Tname` where gender=0; |
| min\(`field`\) | 求此列最小值 | select min\(id\) from `Tname` where isDelete=0; |
| sum\(`field`\) | 对数值列求和 | select sum\(age\) from `Tname` where age&gt;30; |
| avg\(`field`\) | 对数值列求平均值 | select avg\(age\) from `Tname`; |

## 分组

> 分组后，只能查询出相同的数据列,非聚合字段，不能出现在结果集中。  
> select `field`,`field2` from `Tname` group by `field`,`field2`;
>
> * 查询女生人数
>   * select `gender` as `性别`, count\(\*\) from `Tname` group by `gender`;

### 分组后筛选

> having,对分组后的结果集进行筛选。
>
> * select `gender` as `性别`, count\(\*\) as `rs` from `Tname` group by `gender` having `rs`&gt;2;

## 排序

* asc: 升序
* desc: 降序

```sql
select * from `Tname` where isDelete=0 order by `field` asc|desc (, `field` asc|desc);
```

## 分页

* start: 从第几条数据开始，从0算
* count: 取几条

```sql
select * from `Tname` limit `start`, `count`;
```

* 每页显示m条数据，当前是第n页
  * start = \(n-1\)\*m



