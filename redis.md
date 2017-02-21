# 介绍
应用中使用广泛的NoSQL，采用Key-Value形式进行存储。

> 相关资源
- [官方命令教程](http://redis.cn/commands.html)
- [易百教程](http://www.yiibai.com/redis/redis_quick_guide.html)

# 安装
mac
```bash
$ brew install redis
```

Ubuntu
```
$ sudo apt-get install redis-server
```
## 运行
```bash
# 服务端
$ redis-server
# 客户端
$ redis-cli
```

## 配置

## 数据类型(5种)
- 字符串string
- 哈希hash
- 列表list
- 集合set
- 有序集合zset

## string
> string是最基本的类型，几乎什么都能存，最大能存储512MB的数据，string是类型安全的。

命令 | 示例
---| ---
设置 |
设置键值 | SET key value
设置键值及过期时间 | SETEX key seconds value
设置多个键值 | MSET key value [key value ...]
获取 |
根据键获取值 | GET key
根据多个键获取多个值 | MGET key [key ...]
运算(仅对数值) | 
对value+1操作 | INCR key
对value+整数 | INCRBY key increment
对value-1操作 | DECR key
对value-整数 | DECRBY key decrement
其它 | 
追加值 | APPEND key value
获取值长度 | STRLEN key

## 键操作

功能 | 命令
--- | ----
查找键(支持正则) | KEYS pattern
判断键是否存在(有1无0) | EXISTS key [key ...]
查看value类型 | TYPE key
删除键及对应值 | DEL key [key ...]
设置过期时间 | EXPIRE key seconds
查看有效时间(s) | TTL key

## hash
> hash用于存储对象，对象的格式为键值对

 功能 | 命令
 ---- | ----
 设置 | 
 设置单个属性 | HSET key field value
 设置多个属性 | MHSET key field value [field value ...]
 获取 | 
 获取一个属性的值 | HGET key field
 获取多个属性的值 | HMGET key field [field ...]
 获取所有属性和值 | HGETALL key
 获取所有的属性 | HKEYS key
 返回包含属性的个数 | HLEN key
 获取所有值 | HVALS key
 其它 |
 判断值是否存在 | HEXISTS key field
 删除属性及值 | HDEL key field [field ...]
 返回值的字符串长度 | HSTRLEN key field
 
 ## list
 > 
- list列表的元素为string
- 按照插入顺序排序
- 在列表的头部或者尾部添加元素

功能 | 命令
---- | ----
设置 |
左侧插入数据 | LPUSH key value [value ...]
右侧插入数据 | RPUSH key value [value ...]
在指定位置前后插入数据 | LINSERT key BEFORE\|AFTER pivot value
设置索引所在元素的值 | LSET key index value
获取 |
从左侧弹出key对应list的首个元素并返回 | LPOP key
从右侧弹出key对应list的首个元素并返回 | RPOP key
返回列表制定范围内的元素 | LRANGE key start stop
其它 | 
裁剪列表，改为原集合的一个子集 | LTRIIM key start stop
返回list长度 | LLEN key
返回list索引位置的元素 | LINDEX key index

## set
>
- 无序集合
- 元素为string类型
- 元素具有唯一性，不重复

功能 | 命令 
--- | ---
设置 | 
添加元素 | SADD key member [member ...]
获取 |
返回集合所有的元素 | SMEMBERS key
返回集合元素个数 | SCARD key
其它 |
交集 | SINTER key [key ...]
差集 | SDIFF key [key ...]
合集 | SUNION key [key ...]
判断元素是否在集合中 | SISMEMBER key member

## zset
>
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

