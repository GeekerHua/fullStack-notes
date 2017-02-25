shell介绍

终极shell，zsh


todo-list
- [ ] awk的语法
- [ ] grep的完整用法
- [ ] sed的完整用法
- [ ] 指定变量的类型，使用declare或者typeset


# shell数组
- 数组定义使用圆括号，例如
    > array=(1 3 55)
- 数组的读取一般格式
    > ${array[index]}
    > bash中index从0开始，zsh中index从1开始。
- 使用@或*可以获取数组中的所有元素，例如
    > echo ${array[*]}
- 获取数组的长度
    > echo ${#array[*]}

# printf命令
类似C语言的printf()，示例：
```bash
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876 
```

# 输入/输出重定向
> 需要注意的是文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。

重定向命令：
命令 | 说明
--- | ----
command > file | 将输出重定向到file
command < file | 将输入重定向到file
command >> file | 将输出以追加的方式重定向到file
n > file | 将文件描述符为n的文件重定向的到file
n >> file  |  将文件描述符为 n 的文件以追加的方式重定向到 file。
n >& m |  将输出文件 m 和 n 合并。
n <& m |  将输入文件 m 和 n 合并。
<< tag |  将开始标记 tag 和结束标记 tag 之间的内容作为输入。




