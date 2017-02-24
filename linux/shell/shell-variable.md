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

-----
# 变量高级

## 参数替换
### 处理和(或)扩展变量
同长情况下${parameter}与$parameter相同,都是parameter的值。但在某些情况下最好使用前者，避免混淆。
扩展命令 | 说明 
----- | ----
${parameter-default}|	如果变量 parameter 没被声明，那么就使用默认值。
${parameter:-default}|	如果变量 parameter 没被设置，那么就使用默认值。
${parameter=default}|	如果变量parameter没声明，那么就把它的值设为default。
${parameter:=default}|	如果变量parameter没设置，那么就把它的值设为default。
${parameter+alt_value}|	如果变量parameter被声明了，那么就使用alt_value，否则就使用null字符串。
${parameter:+alt_value}|	如果变量parameter被设置了，那么就使用alt_value，否则就使用null字符串。
${parameter?err_msg}|	如果parameter已经被声明，那么就使用设置的值，否则打印err_msg错误消息。
${parameter:?err_msg}|	如果parameter已经被设置，那么就使用设置的值，否则打印err_msg错误消息。 

### 变量长度/子串删除
- ${#var}: 获取字符串长度
- ${#array}: 获取数组中第一个元素的长度
- ${var#Pattern}: 从变量$var的开头删除最短匹配$Pattern的子串。
- ${var##Pattern}: 从变量$var的开头删除最长匹配$Pattern的子串。
- ${var%%Pattern}:从变量$var的结尾删除最短匹配$Pattern的子串。
- ${var%%Pattern}:从变量$var的结尾删除最长匹配$Pattern的子串。
```bash
$ var1=abcd-1234-defg
$ t=${var1#*-*}
# 因为#匹配最短的字符串,
# 同时*匹配任意前缀, 包括空字符串.
$ echo $t
-> 1234-defg
```

### 变量扩展/子串删除



