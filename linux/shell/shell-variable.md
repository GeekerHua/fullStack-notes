# 变量
$variable是${variable}的缩写，前者某些上下文中可能会引起错误。

1. 变量替换

```bash
hello="a b c   d"
# 以下四个结果两两相同，使用"可以保留变量中间的空格和tab
echo $hello
echo ${hello}
echo "$hello"
echo "${hello}"

# '单引号会将$解释为单独的字符
echo '$hello'
# 取消hello变量的赋值
unset hello
```

2. 变量赋值
`=`既可以用来赋值，也可以用来比较

3. 变量不区分类型
bash不区分变量类型

4. 特殊变量
位置参数：从命令行传递到脚本的参数: $0 $1 $2 ... $9 ${10} ${11}
两个特殊的: $*和$@表示所有的位置参数。






