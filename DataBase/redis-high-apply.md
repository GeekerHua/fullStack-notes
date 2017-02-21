# 一、安全性
设置密码的方式有两种:

1. 使用`config set`命令的`requirepass`参数。
    - 格式为： config set requirepass `password`
2. 配置redis.conf中设置requirepass属性，后面为密码。

验证方式也是两种：

1. 登录时`redis-cli -a password`
2. 登录后使用 `auto password`

## 设置密码

1. 使用第一种方式设置密码
```sql
> CONFIG SET requirepass test123
```

2. 使用第二种方式
```bash
$ sudo vim /etc/redis/redis.conf
```
使用vi编辑器修改“# requirepass foobared” 为“requirepass test123”，然后保存退出。

## 重启redis-server和redis-cli
```bash
$ sudo service redis-server restart
```

1. 第一种验证方式
```bash
$ redis-cli -a test123
```

2. 第二种验证方式
```sql
> auth test123
```

# 二、主从复制
Redis的主从复制配置和使用都比较简单，通过主从复制可以允许多个slave server拥有和master server相同的数据库副本。

从服务器只能读，不能写。

**Redis主从复制特点：**

1、master可以拥有多个slave。

2、多个slave可以连接同一个master外，还可以连接到其他的slave。（当master宕机后，相连的slave转变为master）

3、主从复制不会阻塞master，再同步数据时，master可以继续处理client请求。

4、提高了系统的可伸缩性。

**Redis主从复制的过程：**

1、Slave与master建立连接，发送sync同步命令。

2、 Master会启动一个后台进程，将数据库快照保存到文件中，同时Master主进程会开始收集新的写命令并缓存。

3、 后台完成保存后，就将此文件发送给Slave。

4、 Slave将此文件保存到磁盘上。

# 三、事务处理
Redis的事务处理比较简单。只能保证client发起的事务中的命令可以连续的执行，而且不会插入其他的client命令，当一个client在连接中发出multi命令时，这个连接就进入一个事务的上下文，该连接后续的命令不会执行，而是存放到一个队列中，当执行exec命令时，redis会顺序的执行队列中的所有命令。如果其中执行出现错误，执行正确的不会回滚，不同于关系型数据库的事务。

```sql
> multi
> set name a
> set name b
> exec
> get name
```

# 四、持久化
Redis是一个支持持久化的内存数据库，Redis需要经常将内存中的数据同步到磁盘来保证持久化。

Redis支持两种持久化方式：

1、snapshotting（快照），将数据存放到文件里，默认方式。

是将内存中的数据已快照的方式写入到二进制文件中，默认文件dump.rdb，可以通过配置设置自动做快照持久化的方式。可配置Redis在n秒内如果超过m个key被修改就自动保存快照。

save 900 1 #900秒内如果超过1个key被修改，则发起快照保存

save 300 10 #300秒内如果超过10个key被修改，则快照保存

2、 Append-only file（缩写为aof），将读写操作存放到文件中。

由于快照方式在一定间隔时间做一次，所以如果Redis意外down掉的话，就会丢失最后一次快照后的所有修改。
aof比快照方式有更好的持久化性，是由于使用aof时，redis会将每一个收到的写命令都通过write函数写入到文件中当redis启动时会通过重新执行文件中保存的写命令来在内存中重新建立整个数据库的内容。

由于os会在内核中缓存write做的修改，所以可能不是立即写到磁盘上，这样aof方式的持久化也还是有可能会丢失一部分数据。可以通过配置文件告诉redis我们想要通过fsync函数强制os写入到磁盘的时机。