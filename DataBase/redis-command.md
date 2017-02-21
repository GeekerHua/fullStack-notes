# 数据类型(5种)
- 字符串string
    - 字符串是一种最基本的Redis值类型。Redis字符串是二进制安全的，这意味着一个Redis字符串能包含任意类型的数据，例如： 一张JPEG格式的图片或者一个序列化的Ruby对象。一个字符串类型的值最多能存储512M字节的内容。
- 哈希hash
- 列表list
- 集合set
- 有序集合zset

## string
> string是最基本的类型，几乎什么都能存，最大能存储512MB的数据，string是类型安全的。

功能 | 命令 | 示例 | 结果
---| --- | --- | ---
设置 |
设置键值 | SET key value | SET name hua | OK
设置键值及过期时间 | SETEX key seconds value | SETEX age 5 26 | OK
设置多个键值 | MSET key value [key value ...] | MSET gender boy height 166 | OK
获取 |
根据键获取值 | GET key | GET name | "hua"
根据多个键获取多个值 | MGET key [key ...] | MGET gender height| 1) "boy" 2) "166"
运算(仅对数值) |
对value+1操作 | INCR key | INCR age | (integer) 1
对value+整数 | INCRBY key increment | INCRBY age 16 | (integer) 17
对value-1操作 | DECR key | DECR age | (integer) 16
对value-整数 | DECRBY key decrement | DECRBY age 3 | (integer) 13
其它 |
追加值 | APPEND key value | APPEND age 22 | (integer) 4 ,get age ="1322"
获取值长度 | STRLEN key | STRLEN name | (integer) 4

## 键操作(适用于全体类型)

功能 | 命令 | 示例 | 结果
--- | ---- | --- | ---
查找键(支持正则) | KEYS pattern | KEYS * | 1) "name",2) "age"，。。。。
判断键是否存在(有1无0) | EXISTS key [key ...] | EXISTS weight height | (integer) 1
查看value类型 | TYPE key | TYPE age | string
删除键及对应值 | DEL key [key ...] | DEL height | (integer) 1
设置过期时间 | EXPIRE key seconds | EXPIRE gender 5 | "boy",(nil)
查看有效时间(s) | TTL key | TTL name | (integer) -1
随机获取一个已经存在的key | RANDOMKEY | RANDOMKEY | (nil), "name"
修改key的名字，如存在覆盖 | RENAME oldname newname | RENAME name mingzi
修改key的名字，如存在则失败 | RENAMENX oldname newname | RENAMENX name mingzi
清除屏幕 | CLEAR |
返回当前数据库key总数 | DBSIZE
清除当前数据库所有键 | FLUSHDB/FLUSHALL

## hash
> hash用于存储对象，对象的格式为键值对

功能 | 命令 | 示例 | 结果
---- | ---- | --- |---
设置 |
设置单个属性 | HSET key field value | HSET sam python 99 | (integer) 1
设置多个属性 | HMSET key field value [field value ...] | HMSET alice python 99 php 88 | OK
获取 |
获取一个属性的值 | HGET key field | HGET sam python | "99"
获取多个属性的值 | HMGET key field [field ...] | HMGET alice python php | 1) "99", 2) "88"
获取所有属性和值 | HGETALL key | HGETALL alice | 1) "python", 2) "99", 3) "php", 4) "88"
获取所有的属性 | HKEYS key | HKEYS alice | 1) "python", 2) "php"
返回包含属性的个数 | HLEN key | HLEN alice | (integer) 2
获取所有值 | HVALS key | HVALS alice | 1) "99", 2) "88"
其它 |
判断值是否存在 | HEXISTS key field | HEXISTS sam php | (integer) 0
删除属性及值 | HDEL key field [field ...] | HDEL alice python php | (integer) 2
返回值的字符串长度 | HSTRLEN key field | HSTRLEN sam python | (integer) 2
## list
>
- list列表的元素为string
- 按照插入顺序排序
- 在列表的头部或者尾部添加元素

功能 | 命令 | 示例 | 结果
---- | ---- | --- | ---
设置 |
左侧插入数据 | LPUSH key value [value ...] | LPUSH date two one | (integer) 2
右侧插入数据 | RPUSH key value [value ...] | RPUSH date nine ten | (integer) 4
在指定位置前后插入数据 | LINSERT key BEFORE\|AFTER pivot value | LINSERT num AFTER two three | (integer) 5
设置索引所在元素的值 | LSET key index value | LSET num 3 eight | OK
获取 |
从左侧弹出key对应list的首个元素并返回 | LPOP key | LPOP num | "one"
从右侧弹出key对应list的首个元素并返回 | RPOP key | RPOP num | "ten"
返回列表指定范围内的元素 | LRANGE key start stop | LRANGE num 0 5 | 1) "two", 2) "three", 3) "eight"
其它 |
裁剪列表，改为原集合的一个子集 | LTRIM key start stop | LTRIM num 1 2 | OK (1) "three", 2) "eight")
返回list长度 | LLEN key | LLEN num | (integer) 2
返回list索引位置的元素 | LINDEX key index | LINDEX num 1 | "eight"

## set
- 无序集合
- 元素为string类型
- 元素具有唯一性，不重复

功能 | 命令 | 示例 | 结果
--- | --- | --- | ---
设置 |
添加元素 | SADD key member [member ...] | SADD a 1 2 3 4 | (integer) 4
获取 |
返回集合所有的元素 | SMEMBERS key | SMEMBERS a | 1) "1", 2) "2", 3) "3", 4) "4"
返回集合元素个数 | SCARD key | SCARD a | (integer) 4
其它 |
交集 | SINTER key [key ...] | SINTER a b | 1) "1", 2) "3"
差集(a-b,以a为准) | SDIFF key [key ...] | SDIFF a b | 1) "2", 2) "4"
合集 | SUNION key [key ...] | SUNION a b | 1) "a", 2) "c", 3) "3", 4) "1", 5) "2", 6) "4"
判断元素是否在集合中 | SISMEMBER key member | SISMEMBER a 3 | (integer) 1

## zset
- 有序集合
- 元素为string类型
- 元素具有唯一性，不重复
- 每个元素都会关联一个double类型的score，表示权重，通过权重将元素从小到大排序
- 元素的score可以相同

功能 | 命令
--- | ----
设置 |
添加 | ZADD key score member [score member ...]
获取 |
返回指定范围内的元素 | ZRANGE key start stop
返回元素个数 | ZCARD key
返回有序集key中，score值在min和max之间的成员 | ZCOUNT key min max
返回有序集key中，成员member的score值 | ZSCORE key member