## ubuntu下
> mysql默认端口为3306，默认情况只允许本地访问，需要设置才能远程访问。

- 安装
```bash
# ubuntu下
$ sudo apt-get install mysql-server mysql-client
--
# mac下
$ brew install mysql
# 然后按照提示输入
```

### 管理服务
```bash
# ubuntu
$ sudo service mysql start        # 启动
$ sudo service mysql stop         # 停止
$ sudo service mysql restart      # 重启
--
# mac
$ mysql.server start        # 启动
$ mysql.server stop         # 停止
$ mysql.server restart      # 重启
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
> grant all privileges on *.* to 'root'@'%' identified by '数据库密码' with grant option;   # 其中'mysql'为数据库访问密码
> flush privileges;
```

- 重启mysql
```bash
# ubuntu下
$ sudo service mysql restart
--
# mac下
$ mysql.server restart
```

#### 修改数据库密码
mysql登陆成功后
```bash
> SET PASSWORD FOR `root`@`localhost` = PASSWORD('newpass');
```

或终端下
```bash
$ mysqladmin -u root -p password 密码  
```

mac下开启安全模式?????啥是安全模式
```bash
$ mysqld_s?afe --skip-grant-tables
```

## 备份及恢复
### 备份
Ubuntu下
```bash
$ cd /var/lib/mysql 
$ sudo -s
# mysqldump -u root -p DBname > bak.sql
```

### 还原
#### 方法一
```bash
$ cd /var/lib/mysql    #(进入到MySQL库目录，根据自己的MySQL的安装情况调整目录) 
# mysql -u root -p DBname < bak.sql，
```

#### 方法二
- $ mysql -u root -p回车，输入密码，进入MySQL的控制台"mysql>"
- 建立你要还原的数据库，输入"create database DBname；"
- 切换到刚建立的数据库，输入"use DBname；"
- 导入数据，输入"source bak.sql；"