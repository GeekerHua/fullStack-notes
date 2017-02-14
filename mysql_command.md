## 数据库命令

| 功能 | 命令 |
| --- | --- |
| 查看所有数据库 | show databases; |
| 查看当前使用的数据库 | select database\(\); |
| 切换数据库 | use `BDname`; |
| 创建数据库 | create database `DBname` charset=utf8; |
| 删除数据库 | drop database `DBname`; |

## 数据表命令

| 功能 | 命令 |
| --- | --- |
| 查看所有表 | show tables; |
| 创建表 | create table `Tname` \(id int auto\_increment primary key not null, `…………`\); |
| 删除表 | drop talbe `Tname`; |
| 修改表 | alter table `Tname` add\|change\|drop `列`; |

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
* 消除关键字
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
  >
  > | 匹配符 | 匹配内容 |
  > | --- | --- |
  > | `%` | 匹配0到多个任意字符 |
  > | `_` | 匹配一个任意字符 |

* 范围查询

  > in 表示在一个非连续的范围内  
  > between…and…在一个连续的范围内\(between紧挨着的and是它的匹配语句\)
  >
  > ```bash
  > > select * from `Tname` where id in(1,3,8); 
  > > select * from `Tname` where id between 2 and 8;
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
| sum\(`field`\) | 对数值列求和 | select sum\(age\) from `Tname` where age&lt;30; |
| avg\(`field`\) | 对数值列求平均值 | select avg\(age\) from `Tname`; |



