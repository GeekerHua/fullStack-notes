## 安装Mongodb

### Ubuntu下

偶数版本为稳定版本，技术版本为开发版本。详细的安装方式参考官网[https://www.mongodb.com](https://www.mongodb.com)，不同平台不同版本的安装方式都不相同。

### Mac下

```bash
$ brew install mongodb # 安装mongodb
$ sudo mkdir -p /data/db # mongodb默认存储位置为/data/db
$ sudo chown hua /data/db # 修改该文件夹权限，以便mongodb能够有权限访问
```

## 安装robomongo

> robomongo是一个mongodb的可视化工具，具有多个平台的版本。

mac下安装

```bash
$ brew cask install robomongo
```

## 服务端设置

ubuntu下，设置文件位置，/etc/mongod.conf，默认端口27017

服务端操作

* Ubuntu下

```bash
# 启动
$ sudo service mongod start
# 停止
$ sudo service mongod stop
# 重启
$ sudo service mongod restart
```

* Mac下

```
$ mongd
```

## 客户端设置

启动客户端

```bash
$ mongo
```

## 安全

## 复制(副本集)

## 备份和恢复