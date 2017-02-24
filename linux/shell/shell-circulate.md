## for循环
```bash
#!/bin/bash 
# 列出所有的行星名称. (译者注: 现在的太阳系行星已经有了变化^_^) 
for planet in Mercury Venus Earth Mars Jupiter Saturn Uranus Neptune Pluto 
do 
    echo $planet # 每个行星都被单独打印在一行上. 
done 
echo 

for planet in "Mercury Venus Earth Mars Jupiter   Saturn Uranus Neptune Pluto" 
# 所有的行星名称都打印在同一行上. 
# 整个'list'都被双引号封成了一个变量. 
do 
  echo $planet 
done 
exit 0
```

## while循环
```bash
#!/bin/bash
var0=0
LIMIT=10

while [ "$var0" -lt "$LIMIT" ]
do
  echo -n "$var0 "        # -n 将会阻止产生新行.
  var0=`expr $var0 + 1`   # var0=$(($var0+1))  也可以.
  # var0=$((var0 + 1)) 也可以.
  # let "var0 += 1"    也可以.
done                      # 使用其他的方法也行.

echo
exit 0
```

## until循环
```bash
#!/bin/bash 
END_CONDITION=end
until [ "$var1" = "$END_CONDITION" ] 
# 在循环的顶部进行条件判断. 
do 
  echo "Input variable #1 " 
  echo "($END_CONDITION to exit)" 
  read var1 
  echo "variable #1 = $var1" 
  echo 
done 

exit 0
```

# 循环控制
## break
## continue

# 测试与分支
## case(in)/esac
> 在shell中的case结构与C/C++中的switch结构是相同的。它允许通过判断来选择代码块中多 条路径中的一条。它的作用和多个if/then/else语句的作用相同，是它们的简化结构，特别 适用于创建菜单。case块以esac（case的反向拼写）结尾。

```bash
#!/bin/bash 
# 测试字符串范围. 
echo; echo "Hit a key, then hit return." 
read Keypress 

case "$Keypress" in 
[[:lower:]] ) echo "Lowercase letter";; 
[[:upper:]] ) echo "Uppercase letter";; 
[0-9] ) echo "Digit";; 
* ) echo "Punctuation, whitespace, or other";; 
esac 
# 允许字符串的范围出现在[中括号]中, 
#+ 或者出现在POSIX风格的[[双中括号中. 
# 在这个例子的第一个版本中, 
#+ 测试大写和小写字符串的工作使用的是 
#+ [a-z] 和 [A-Z]. 
# 这种用法在某些特定场合的或某些Linux发行版中不能够正常工作. 
# POSIX 的风格更具可移植性. 
exit 0
```
## select
> select结构是建立菜单的另一种工具，这种结构是从ksh中引入的。

```bash
#!/bin/bash 
PS3='Choose your favorite vegetable: ' # 设置提示符字串. 
echo 

select vegetable in "beans" "carrots" "potatoes" "onions" "rutabagas" 
do 
  echo 
    echo "Your favorite veggie is $vegetable." 
    echo "Yuck!" 
    echo break # 如果这里没有 'break' 会发生什么? 
done 

exit 0
```
