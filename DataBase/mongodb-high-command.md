# 聚合aggregate
主要用于计算数据，类似sql中的sum()、avg()
- 语法： db.`Tname`.aggregate([{管道: {表达式}}])

## 常用管道
管道名 | 说明
------ | -----
$group | 将集合中的文档分组，可用于统计结果
$match | 过滤数据，只输出符合符合条件的文档
$project | 投影,字段为1显示，字段为0不显示,修改输入文档的结构，如重命名、增加、删除字段、创建计算结果
$sort    | 将输入文档排序后输出，1升序，-1降序
$limit    |限制聚合管道返回的文档数
$skip    | 跳过制定数量的文档，并返回余下的文档
$unwind  | 将数组类型的字段进行拆分
$geoNear | 输出接近某一地理位置的有序文档

## 常用表达式
> 语法： '$`列名`'

表达式 | 说明
------ | -----
$sum    | 计算总和，`$sum:1` 同count表示计数
$avg    | 计算平均值
$min    | 获取最小值
$max    | 获取最大值
$push    | 在结果文档中插入值到一个数组中
$addToSet | 在结果文档中插入值到一个数组，但不创建副本
$first    | 根据资源文档的排序获取第一个文档数据
$last    | 根据资源文档的排序获取最后一个文档数据

## $group
```sql
db.stu.aggregate([
    {$group:
        {
            _id:'$genter',
            counter: {$sum:1},
            avg: {$avg: '$age'},
            list: {$push: '$$ROOT'}
        }
    }
])
```

## $match
```sql
db.stu.aggregate([
    {$match:{age:{$gt:20}}},
    {$group:{_id:'$gender',counter:{$sum:1}}}
])
```

## $project
```sql
db.stu.aggregate([
    {$group:{_id:'$gender',counter:{$sum:1}}},
    {$project:{_id:0,counter:1}}
])
```

## $sort
```sql
db.stu.aggregate([
    {$group:{_id:'$gender',counter:{$sum:1}}},
    {$sort:{counter:-1}}
])
```

## $limit、skip
```sql
db.stu.aggregate([
    {$group:{_id:'$gender',counter:{$sum:1}}},
    {$sort:{counter:1}},
    {$skip:1},
    {$limit:1}
])
```

## $unwind
> 将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值
- db.集合名称.aggregate([{$unwind:'$字段名称'}])
- db.t3.aggregate([{$unwind:{path:'$sizes', preserveNullAndEmptyArrays:true}}])

> preserveNullAndEmptyArrays: `boolean`  #防止数据丢失,如果是false，对于空数组、无字段、null的文档，会被丢弃。

# 索引
语法
```sql
db.COLLECTION_NAME.ensureIndex({KEY:1|-1})
```
> 1代表升序、-1代表降序

## ensureIndex()的可选参数：

参数|	类型|	描述
--- | --- | ---
background|	Boolean	建立索引要不要阻塞其他数据库操作，默认为false
unique|	Boolean|	建立的索引是否唯一，默认false|
name|	string|	索引的名称，若未指定，系统自动生成
dropDups|	Boolean|	建立唯一索引时，是否删除重复记录，默认flase
sparse|	Boolean|	对文档不存在的字段数据不启用索引，默认false
expireAfterSeconds|	integer|	设置集合的生存时间，单位为秒
v|	index version|	索引的版本号
weights|	document|	索引权重值，范围为1到99999
default-language|	string|	默认为英语
language_override|	string|	默认值为 language

范例
```sql
db.shiyanlou.ensureIndex({"user_id":1,"name":1},{background:1})
```
