# 数据库命令
- 查看所有数据库: show databases;
- 查看当前使用的数据库: select database();
- 切换数据库: use `BDname`;
- 创建数据库: create database `DBname` charset=utf8;
- 删除数据库: drop database `DBname`;


# 数据表命令
- 查看所有表: show tables;
- 创建表: create table `Tname` (id int auto_increment primary key not null, `…………`);
- 删除表: drop talbe `Tname`;
- 修改表: alter table `Tname` add|change|drop `列`;


# 数据命令crud:
- 查询: select * from `Tname`;
- 增加: insert into `Tname` values(……);
- 修改: update `Tname` set `key`=`value`;
- 删除: delete from `Tname` where `……`;
- 逻辑删除: 本质就是修改
    - alter table `Tname` add isDelete bit default 0;
    - update `Tname` isDelete=1 where `……`;
    





