## 字符串函数

函数 | 功能 | 示例
-----| ----| ----
ascii |查看字符的ascii码值 | select ascii('a')
char  |查看ascii码对应的字符char | select char(97)
concat | 拼接字符串 | select concat(12,35,'ab')
length  | 包含字符个数 | select length('abc')
left/right(str,len)| 从一侧截取字符串 | select left('afd23',2)
substring(str,pos,len) | 返回字符串str的位置pos(从1起)起len个字符 | select substring('abc124',2,4)
ltrim/rtrim(str) | 返回删除了一边空格的字符串str | select ltrim('   ba')
trim(leading/both/trailing remstr FROM str) | 返回某侧删除remstr后的字符串str | select trim(both 'is' from 'is a tim is')
space(n) | 返回有n个空格组成的空格字符串 | select space(3)
lower(str) | 字符串转为小写 | select lower('hhahaTTTDF')
upper(str) | 字符串转为大写 | select upper('ddTDHddsc')


## 数学函数

函数 | 功能 | 示例
----|------|----
abs | 求绝对值 | select abs(-23)
mod(m,n) | m除以m的余数，等同于% | select mod(10,3)
floor(n) | 地板，不大于n的最大整数 | select floor(2.3)
ceiling(n) | 天花板，不小于n的最大整数 | select ceiling(2.3)
round(n) | 四舍五入 | select round(1,6)
pow(x,y) | 求x的y次幂 | select pow(2,3)
PI() | 获取圆周率 | select PI()
rand() | 随机数0-1.0的浮点数 | select rand()

## 日期时间函数
语法 | 说明
--- | ---
year(date)|返回date的年份(范围在1000到9999)
month(date)|返回date中的月份数值
day(date)|返回date中的日期数值
hour(time)|返回time的小时数(范围是0到23)
minute(time)|返回time的分钟数(范围是0到59)
second(time)|返回time的秒数(范围是0到59)

```sql
select year('2016-12-21');
```
### 日期计算
> 使用+-运算符，数字后面的关键字为year、month、day、hour、minute、second

```sql
select '2016-12-21'+interval 1 day;
```
### 日期格式化
> date_format(date,format)

符号 | 说明
---| ---
获取年%Y|返回4位的整数
获取年%y|返回2位的整数
获取月%m|值为1-12的整数
获取日%d|返回整数
获取时%H|值为0-23的整数
获取时%h|值为1-12的整数
获取分%i|值为0-59的整数
获取秒%s|值为0-59的整数

```sql
select date_format('2016-12-21','%Y %m %d');
```
函数 | 用法
--- | ---
当前日期current_date() | select current_date();
当前时间current_time() | select current_time();
当前日期时间now() | select now();

## 类型转换函数
> 有cast和convert两个函数
- cast(value as type)
- convert(value, type)
> value表示要转换的值
> type表示目标类型


- 二进制binary
- 字符型char，可指定字符个数如char(10)
- 日期date
- 时间time
- 日期时间型datetime
- 浮点数decimal
- 整数signed
- 无符号整数unsigned

```sql
SELECT CONVERT('125.83',SIGNED);
SELECT CAST('125.83' AS signed);
```

