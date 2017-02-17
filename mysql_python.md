# Python交互

## connection对象
connection对象默认开启了事务，执行完sql语句后需要执行commit操作。

### 方法
方法名 | 作用 
----  | ----
close() | 关闭connection对象
commit() | 提交事务
rollback() |　事务回滚操作
cursor()　| 返回Cursor对象，使用Cursor执行sql语句。

## Cursor对象
### 方法
方法名      | 作用 
----       | ---
close()    | 关闭Cursor对象
execute('sql', [args])  | 执行sql语句，可以加参数,使用`%s`占位
fetchone() | 获取查询结果第一行数据，返回元组
next()     | 获取查询结果集的下一行数据
fetchall() | 获取结果集的所有行，返回二元元组
scroll()   | 将指针移动到某个位置

### 属性
- rowcount: 表示最近一次execute()执行后受影响的行数
- connection: 获得当前Connection对象

## 代码
```python
from MySQLdb import *

try:
    coon = connect(host='localhost', port=3306, user='root', passwd='1234qwer', db='python3', charset='utf8')    # 连接数据库
    cursor1 = coon.cursor()        # 创建Cursor光标

    sql = "insert into students(name) values('%s')"
    cursor1.execute(sql, 'laowang')    # 查询语句
    coon.commit()        # 提交事务

    cursor1.close()    # 关闭Cursor
    coon.close()       # 关闭connection
except Exception, e:
    print(e.message)
```