

#　NoSQL
Not Only SQL。即非关系型数据库，不维护数据库。

# MongoDB
MongoDB基于分布式 文件存储的NoSQL数据库

## 安装Mongodb
### Ubuntu下

### Mac下
brew install mongodb
sudo mkdir -p /data/db
sudo chown hua /data/db

### 安装robomongo
> robomongo是一个mongodb的可视化工具，具有多个平台的版本。

mac下安装
```bash
$ brew cask install robomongo
```

如何使用service启动mongod
如何修改mongodb的设置mongod.conf


## Mongodb的特点
三元素
- 数据库
- 集合： 表
- 文档： 行

## 基础命令
命令 | 说明
---- | ----
db	| 显示当前数据库
show dbs | 显示所有数据库
use `Tname` | 创建数据库，用数据库


## 集合创建
命令 | 说明
---- | ----
db.createCollection(“sub”) | 创建集合
db.sub.drop(). | 删除集合
show collections | 查看集合

## 文档操作
####插入

#### 更新
sb.stu.update(<query>,<update>,{multi:<boolean>})
- 更新一条数据
db.stu.update({name:'hr'},{name:'mnc'})
- 指定属性更新，通过操作符$set
db.stu.update({name:'hr'},{$set:{name:'hys'}})
- 修改多条匹配到的数据
db.stu.update({},{$set:{gender:0}},{multi:true})

#### 保存
>  如果有_id则修改数据，没有_id则添加文档

db.`Tname`.save(document)

#### 删除
db.`Tname`.remove({}, {justOne: <boolean>})

