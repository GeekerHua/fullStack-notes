  ## 实体关系
- 1对1：存在那个表都行
- 1对多：存在多的那个表
- 多对多： 存在一张新表中

## 外键
- foreign key(`field`) references `Tname`(`primary key`);

### 外键的级联操作
> 当表中数据被删除了，这个表被其他表外键关联的时候。

- restrict(限制): 默认值，抛异常
- cascade(级联): 如果主表记录删掉，则从表中相关联的记录都将被删除
- set null:将外键设置为空
- no action:什么都不做
> 推荐使用逻辑删除

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

## 自关联查询
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