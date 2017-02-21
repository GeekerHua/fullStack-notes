# 介绍
应用中使用广泛的NoSQL，采用Key-Value形式进行存储，属于内存级存储。

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


## 设置相关命令
### 查询信息
INFO [section] ：查询Redis相关信息。 INFO命令可以查询Redis几乎所有的信息，其命令选项有如下：

section | 功能
----- | ----
server |Redis server的常规信息
clients| Client的连接选项
memory|存储占用相关信息
persistence| RDB and AOF 相关信息
stats| 常规统计
replication| Master/slave请求信息
cpu |CPU 占用信息统计
cluster| Redis 集群信息
keyspace| 数据库信息统计
all| 返回所有信息
default| 返回常规设置信息
