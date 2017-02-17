# 安装

> mysql默认端口为3306，默认情况只允许本地访问，需要设置才能远程访问。

- 安装
```bash
# ubuntu下
$ sudo apt-get install mysql-server mysql-client
--
# mac下
$ brew install mysql
# 然后按照提示输入
--
# pip2中
$ pip install MySQL-python
--
# pip3中 (python3中不再对`MySQL-python`进行支持)
$ pip3 install PyMySQL
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

#### mac下配置
[参考链接](https://segmentfault.com/q/1010000000475470)
> 配置文件位置 `/usr/local/opt/mysql/bin/mysql_secure_installation` //mysql 提供的配置向导

会出现以下几个问题
- Would you like to setup VALIDATE PASSWORD plugin?(是否使用认证的密码，这里系统会对密码强度进行限制)
- Please set the password for root here.(设置root密码)
- Do you wish to continue with the password provided?(是否确定使用刚才提供的密码)
- Remove anonymous users?(是否移除匿名用户)
- Disallow root login remotely?(是否禁止远程登录)
- Remove test database and access to it?(是否删除测试数据库)
- Reload privilege tables now?(是否重新载入权限表)

> 设置 MySQL 用户以及数据存放地址(未验证)[参考链接](http://blog.csdn.net/wdsdsdsds/article/details/51983453)
```bash
$ unset TMPDIR
$ mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp
```
 
#### ubuntu下
设置文件位置在`/etc/mysql/mysql.conf.d/mysqld.cnf`

##### 设置允许远程连接
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

### 修改数据库密码
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
mac 下
```bash
$ mysqldump -p DBname > bak.sql
```
> 如果备份提示没有权限，使用`sudo`

### 还原
#### 方法一
```bash
$ cd /var/lib/mysql    #(进入到MySQL库目录，根据自己的MySQL的安装情况调整目录) 
# mysql -u root -p DBname < bak.sql，
```

#### 方法二
- $ mysql -u root -p回车，输入密码，进入MySQL的控制台"mysql>"
- 建立你要还原的数据库，输入"create database `DBname`；"
- 切换到刚建立的数据库，输入"use `DBname`；"
- 导入数据，输入"source `bak.sql`；"