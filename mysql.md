## ubuntu下
> mysql默认端口为3306，默认情况只允许本地访问，需要设置才能远程访问。

- 安装
```bash
# ubuntu
$ sudo apt-get install mysql-server mysql-client

# mac
$ brew install mysql
# 然后按照提示输入
```

### 管理服务
```bash
# ubuntu
$ sudo service mysql start    # 启动
$ sudo service mysql stop    # 停止
$ sudo service mysql restart  # 重启

# mac
$ mysql.server start    # 启动
$ mysql.server stop        # 停止
$ mysql.server restart    # 重启

```

### 设置
ubuntu下
设置文件位置在`/etc/mysql/mysql.conf.d/mysqld.cnf`

mac下

#### 设置允许远程连接
- 修改配置文件
```bash
$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
# 将bind-address=127.0.0.1注释
```

- 登录mysql
```bash
$ mysql -u root -p
```

- 修改权限
```bash
$ grant all privileges on *.* to 'root'@'%' identified by 'mysql' with grant option;   # 其中'mysql'为数据库访问密码
$ flush privileges;
```

- 重启mysql
```bash
$ sudo service mysql stop
```

