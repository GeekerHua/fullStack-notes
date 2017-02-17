## 实体关系
- 1对1：存在那个表都行
- 1对多：存在多的那个表
- 多对多： 存在一张新表中

## 外键
> 外键用来对表的数据进行约束。拥有外键的表添加数据，外键值必须是外键所关联的表中存在的数据，否则添加失败。
- foreign key(`field`) references `Tname`(`primary key`);
  
### 外键的级联操作
> 当表中数据被删除了，这个表被其他表外键关联的时候。

- restrict(限制): 默认值，抛异常
- cascade(级联): 如果主表记录删掉，则从表中相关联的记录都将被删除
- set null:将外键设置为空
- no action:什么都不做
> 推荐使用逻辑删除

### 外键的添加及删除(在没有外键的两个表中，重新添加外键。)
- 单独添加外键
    - alter table `Tname` add constraint `外键名` foreign key(`field`) references `Tname`(`field`);
- 单独删除外键
    > 可以用show create `Tname`; 查看外键名

    - alter table `Tname` drop foreign key `外键名`;

> 当需要插入大量有外键数据时，先删除外键，再插入数据，最后添加外键。

## 连接查询

> select students.name, subjects.name,scores.score
from scores
> - inner **join** students **on** scores.stuid=students.id
> - inner **join** subjects **on** scores.subid=subjects.id
;

- A inner join B (内连接)	: 都有的才会出现
- A left join B	(左连接)	: 左边为准
- A right join B (右连接)	: 右表为准

### 示例
```bash
> select students.name,avg(scores.score) from scores  
    inner join students on students.id=scores.stuid 
    inner join subjects on subjects.id=scores.subid 
    group by students.id;
```

## 完整的select语句
```bash
select distinct *
from 表名
where ....
group by ... having ...
order by ...
limit star,count
```

## 执行顺序为：
```bash
from 表名
where ....
group by ...
select distinct *
having ...
order by ...
limit star,count
```

## 自关联
> 一张物理表，包含多张逻辑表，该表的外键关联该表的主键。通常用来解决一张表存储的数据很少，而又有多张结构相似的表，常用在省份、城市地区的数据表中。

```bash
> create table areas(
    aid int primary key,
    atitle varchar(20),
    pid int,
    foreign key(pid) references areas(id)
    );
```

> 自关联查询

```bash
> select city.* from areas as city
    inner join areas as province on city.pid=province.aid
    where province.atitle='山西省';
```

## 视图
> 类似于使用变量对sql语句的封装，视图的功能就是用来查询。视图的结果集用来查询。格式为
create view `Vname` as `sql语句`
使用 select * from `Vname`

```
> create view v_stu_sub_sco as
    select students.*,scores.score from scores
    inner join students on scores.stuid=students.id;
```

## 事务

事务的四大特性
- 原子性(Atomicity)：事务中的全部操作在数据库中是不可分割的，要么全部完成，要么均不执行
- 一致性(Consistency)：几个并行执行的事务，其执行结果必须与按某一顺序串行执行的结果相一致
- 隔离性(通过加锁实现)(Isolation)：事务的执行不受其他事务的干扰，事务执行的中间结果对其他事务必须是透明的
- 持久性(Durability)：对于任意已提交事务，系统必须保证该事务对数据库的改变不被丢失，即使数据库出现故障

> 表的类型必须是innode或bdb类型，才可以对此表使用事务.
- 什么情况下使用事务：
修改、删除、插入，对数据进行修改的时候。

修改表的类型
```bash
> alter table 'Tname' engine=innodb;
```

- begin: 开始，之后的操作是内存级的，并对数据进行锁定。
- commit: 提交，将begin后的操作应用到物理数据库，并解除锁定。
- rollback: 回滚，begin之后的操作取消，并解除锁定。

## 索引
### 选择列的数据类型(提高搜索效率)
- 越小的数据类型通常更好：越小的数据类型通常在磁盘、内存和CPU缓存中都需要更少的空间，处理起来更快
- 简单的数据类型更好：整型数据比起字符，处理开销更小，因为字符串的比较更复杂
- 尽量避免NULL：应该指定列为NOT NULL，除非你想存储NULL。在MySQL中，含有空值的列很难进行查询优化，因为它们使得索引、索引的统计信息以及比较运算更加复杂。你应该用0、一个特殊的值或者一个空串代替空值

### 索引操作
> 索引分单列索引和组合索引
>> 单列索引，即一个索引只包含单个列，一个表可以有多个单列索引，但这不是组合索引
>> 组合索引，即一个索包含多个列

- 查看索引：
```bash
> SHOW INDEX FROM table_name;
```

- 创建索引：
```bash
> CREATE INDEX indexName ON mytable(username(length));
```

- 查看索引：
```bash
> show index from `Tname`;
```

- 删除索引：
```bash
> DROP INDEX [indexName] ON mytable;
```

- 开启运行时间监测：
```bash
> set profiling=1;
```

- 执行查询语句：
```bash
> select * from areas where atitle='北京市';
```

- 查看执行的时间：
```bash
> show profiles;
```
