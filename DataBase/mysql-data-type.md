# mysql的数据类型

数据类型	|大小(字节)|	用途|	格式
--- | --- | --- | ---
INT|	4|	整数	|
FLOA|T	4|	单精度浮点数	|
DOUBLE|	4|	双精度浮点数	|
ENUM	|	|单选,比如性别	|ENUM('a','b','c')
SET|	| 多选|	SET('1','2','3')
DATE	|3	|日期|	YYYY-MM-DD
TIME	|3	|时间点或持续时间|	HH:MM:SS
YEAR	|1	|年份值|	YYYY 
CHAR	|0~255	|定长字符串	|
VARCHAR	|0~255|	变长字符串	|
TEXT	|0~65535|	长文本数据|

# mysql的约束

##约束分类
约束类型：|	主键|	默认值|	唯一|	外键|	非空
-- |--|--|--|--|--
关键字：|	PRIMARY KEY|	DEFAULT|	UNIQUE|	FOREIGN KEY|	NOT NULL

### 主键
主键可以由单列标识，也可以设置符合主键，用多列共同标识。
```sql
id Int(10) PRIMARY KEY
CONSTRAINT proj_pk PRIMARY KEY (proj_num, proj_name)
```

### 唯一约束
```sql
UNIQUE (phone)
```

### 外键约束
```sql

CONSTRAINT emp_fk FOREIGN KEY(in_dpt) REFERENCES department(dpt_name);
```
其中emp_fk为自定义的外键名。