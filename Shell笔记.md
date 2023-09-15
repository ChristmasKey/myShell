# Shell笔记

![首图](./images/首图.jpg)



## 前言

[什么是Shell？](https://www.jianshu.com/p/488dcf2a7a81)

> Shell是一个命令解释器，它在操作系统的最外层，负责直接与用户进行对话，
>
> 把用户的输入解释给操作系统，并处理各种各样的操作系统的输出结果，输出到屏幕反馈给用户。



Shell的作用

- 解释执行用户输入的命令或程序等
- 用户输入一条命令，shell就解释一条
- 键盘输入命令，Linux给予响应的方式，称之为交互式

![Shell作用原理](./images/Shell作用原理.png)

shell是一个包裹着系统核心的**壳**，处于操作系统的最外层，与用户直接对话，

把用户的输入`解释`给操作系统，然后处理操作系统的输出结果，输出到屏幕给用户看到结果。



Shell还是一个功能相当强大的编程语言，易编写、易调试、灵活性强。



## Shell解析器

1、Linux提供的Shell解析器有6种：

```shell
[root@192 ~]# cat /etc/shells
```

![Linux提供的Shell](./images/Linux提供的Shell.png)



2、**bash**和**sh**的关系：

```shell
[root@192 bin]# ll | grep bash
```

![bash和sh的关系](./images/bash和sh的关系.png)



3、CenterOS默认的解析器是bash：

```shell
[root@192 bin]# echo $SHELL
```

![CentOS默认解析器](./images/CentOS默认解析器.png)



## Shell脚本入门

### 1.脚本格式

脚本以 <span style="color:red;">#!/bin/bash</span> 开头（指定解析器）



### 2.HelloWorld

需求：创建一个Shell脚本，输出 HelloWorld

①创建并编辑脚本

```shell
[root@192 ShellScripts]# touch helloworld.sh
[root@192 ShellScripts]# vim helloworld.sh
```

②脚本内容如下

```sh
#!/bin/bash
echo "Hello World Spring Stone"
```

③执行脚本

```shell
[root@192 ShellScripts]# sh helloworld.sh
```

![helloworld脚本执行结果](./images/helloworld脚本执行结果.png)



#### 脚本的常用执行方式

第一种：采用 bash或sh + 脚本的相对路径或绝对路径（不用赋予脚本+x权限）

- sh+脚本的相对路径

```shell
[root@192 ShellScripts]# sh helloworld.sh
```

- sh+脚本的绝对路径

```shell
[root@192 ShellScripts]# sh /data/ShellScripts/helloworld.sh
```

- bash+脚本的相对路径

```shell
[root@192 ShellScripts]# bash helloworld.sh
```

- bash+脚本的绝对路径

```shell
[root@192 ShellScripts]# bash /data/ShellScripts/helloworld.sh
```



第二种：采用输入脚本的绝对路径或相对路径执行脚本（<span style="color:red;">必须具有可执行权限+x</span>）

<span style="color:green;">首先要赋予helloworld.sh脚本的+x权限</span>

```shell
[root@192 ShellScripts]# chmod 777 helloworld.sh
```

执行脚本

- 相对路径

```shell
[root@192 ShellScripts]# ./helloworld.sh
```

- 绝对路径

```shell
[root@192 ShellScripts]# /data/ShellScripts/helloworld.sh
```



<span style="color:red;">**注意：**第一种执行方法，本质是bash解析器帮你执行脚本，所以脚本本身不需要执行权限；第二种执行方法，本质是脚本需要自己执行，所以需要执行权限</span>



### 3.多命令处理

需求：在/data/ShellScripts/目录下创建一个banzhang.txt，在banzhang.txt文件中增加“I love cls”

①创建并编辑脚本

```shell
[root@192 ShellScripts]# touch banzhang.sh
[root@192 ShellScripts]# vim banzhang.sh
```

②脚本内容如下

```sh
#!/bin/bash
cd /data/ShellScripts/
touch banzhang.txt
echo "I love cls" >> banzhang.txt
```

③执行脚本

```shell
[root@192 ShellScripts]# bash banzhang.sh
```

![banzhang脚本执行结果](./images/banzhang脚本执行结果.png)



## Shell中的变量

### 1.系统变量

常用系统变量：

`$HOME`、`$PWD`、`$SHELL`、`$USER`等

查看系统变量的值

```shell
[root@192 ShellScripts]# echo $HOME
[root@192 ShellScripts]# echo $PWD
[root@192 ShellScripts]# echo $SHELL
[root@192 ShellScripts]# echo $USER
```

![查看系统变量](./images/查看系统变量.png)

显示当前Shell中所有变量： set

```shell
[root@192 ShellScripts]# set
```

**由于输出内容过多，请勿轻易使用此命令！！！**



### 2.自定义变量

#### 基本语法

> 1、定义变量：变量=值
>
> 2、撤销变量：unset 变量
>
> 3、声明静态变量：readonly 变量名，<span style="color:red;">注意：不能unset</span>



变量定义规则：

1、变量名称可以由字母、数组和下划线组成，但是不能以数字开头，<span style="color:red;">环境变量名建议大写</span>。

2、<span style="color:red;">等号两侧不能有空格</span>。

3、在**bash**中，变量默认类型都是字符串类型，无法直接进行数值运算。

4、变量的值如果有空格，需要使用双引号或单引号括起来。



#### 实操案例

①自定义一个变量并打印它的值，然后给A重新赋值

```shell
[root@192 ShellScripts]# A=1
[root@192 ShellScripts]# echo $A
[root@192 ShellScripts]# A=8
[root@192 ShellScripts]# echo $A
```

==注意：等号两侧不能有空格！！！==



②撤销定义的变量

```shell
[root@192 ShellScripts]# unset A
```



③声明一个静态变量

```shell
[root@192 ShellScripts]# readonly B=2
[root@192 ShellScripts]# echo $B
```

==注意：静态变量不能被 unset 撤销！！！==



④在bash中，变量默认类型都是字符串类型，无法直接进行数值运算

```shell
[root@192 ShellScripts]# C=1+2
[root@192 ShellScripts]# echo $C
```



⑤为变量赋值含有空格的字符串

```shell
[root@192 ShellScripts]# C="xuezhang love mm"
[root@192 ShellScripts]# echo $C
```



⑥可以把变量提升为全局环境变量，可供其他Shell程序使用

<span style="color:red;">语法：export 变量名</span>

```shell
[root@192 ShellScripts]# vim helloworld.sh

# 在helloworld.sh文件中增加echo $B
#!/bin/bash

echo "Hello World Spring Stone"
echo $B

[root@192 ShellScripts]# export B
[root@192 ShellScripts]# ./helloworld.sh
```



### 3.特殊变量：$n

#### 基本语法

功能描述：n为数字，\$0代表该脚本名称；

\$1~$9代表第一到第九个参数；

十及以上的参数需要用大括号包含表示，如\${10}



#### 实操案例

①输出该脚步的文件名称、输入参数1和输入参数2的值

```shell
[root@192 ShellScripts]# touch parameter.sh
[root@192 ShellScripts]# vim parameter.sh

# 脚本内容如下
#!/bin/bash
echo "$0 $1   $2"

[root@192 ShellScripts]# chmod 777 parameter.sh
[root@192 ShellScripts]# bash parameter.sh 1 100
```



### 4.特殊变量：$#

#### 基本语法

功能描述：获取所有输入参数的个数，常用于循环



#### 实操案例

```shell
[root@192 ShellScripts]# vim parameter.sh

# 脚本内容如下
#!/bin/bash
echo "$0 $1  $2"
echo $#

[root@192 ShellScripts]# bash parameter.sh cls xz
```



### 5.特殊变量：\$*、$@

#### 基本语法

\$*：这个变量代表命令行中所有的参数，\$\*把所有的参数看成一个整体

\$@：这个变量也代表命令行中所有的参数，不过\$@把每个参数区分对待



#### 实操案例

```shell
[root@192 ShellScripts]# vim parameter.sh

# 脚本内容如下
#!/bin/bash
echo "$0 $1  $2"
echo $#
echo $*
echo $@

[root@192 ShellScripts]# bash parameter.sh 1 2 3
```



### 6.特殊变量：$?

#### 基础语法

功能描述：最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一次命令正确执行；如果这个变量的值为非0（具体是哪个数字，有命令自己来决定），则证明上一个命令执行不正确了。



#### 实操案例

判断helloworld.sh脚本是否正确执行

```shell
[root@192 ShellScripts]# ./helloworld.sh
[root@192 ShellScripts]# echo $?
```



## 运算符

### 基本语法

（1）“\$((运算式))” 或 “\$[运算式]”

（2）expr 运算符（+，-，*，/，%）

<span style="color:red;">注意：expr和运算符间要有空格</span>



### 实操案例

计算2+3、3-2

```shell
[root@192 ShellScripts]# expr 2 + 3

[root@192 ShellScripts]# expr 3 - 2
```



复杂计算：（2+3）×4

```shell
[root@192 ShellScripts]# expr `expr 2 + 3` \* 4

[root@192 ShellScripts]# s=$[(2+3)*4]
[root@192 ShellScripts]# echo $s
```



## 条件判断

### 基本语法

[ condition ]（<span style="color:red;">注意condition前后要有空格</span>）

==tip：条件非空即为true，[ spring ]返回true，[ ]返回false==



### 常用判断条件

#### 1.两个整数之间比较

`=` 字符串比较

`-lt` 小于 *less than*；	 	  `-le` 小于等于 *less equal*

`-eq` 等于 *equal*；				`-ne` 不等于 *Not equal*

`-gt` 大于 *greater than*；	`-ge` 大于等于 *greater equal*



#### 2.按照文件权限进行判断

`-r` 有读的权限 *read*

`-w` 有写的权限 *write*

`-x` 有执行的权限 *execute*



#### 3.按照文件类型进行判断

`-f` 文件存在并且是一个常规的文件 *file*

`-e` 文件存在 *existence*

`-d` 文件存在并且是一个目录 *directory*



### 实操案例

①判断23是否大于等于22

```shell
[root@192 ShellScripts]# [ 23 -ge 22 ]
[root@192 ShellScripts]# echo $?
```



②判断helloworld.sh是否具有写权限

```shell
[root@192 ShellScripts]# [ -w helloworld.sh ]
[root@192 ShellScripts]# echo $?
```



③判断/data/ShellScripts/cls.txt文件是否存在

```shell
[root@192 ShellScripts]# [ -e /data/ShellScripts/cls.txt ]
[root@192 ShellScripts]# echo $?
```



④多条件判断

&&表示上一条命令执行成功后，才执行下一条命令

||表示上一条命令执行失败后，才执行下一条命令

```shell
[root@192 ShellScripts]# [ condition ] && echo OK || echo 'not OK'
[root@192 ShellScripts]# [ condition ] && [ ] || echo 'not OK'
```



## 流程控制

### 1.if判断

#### 基本语法

```shell
if[ 条件判断式 ];then
	#程序
fi
```

或者

```shell
if[ 条件判断式 ]
	then
		#程序
fi
```

==注意事项：==

（1）[ 条件判断式 ]，中括号和条件判断式之间必须有空格

（2）<span style="color:red;">if后要有空格</span>



#### 实操案例

输入一个数字，如果是**1**，则输出 **banzhang zhen shuai**；如果是**2**，则输出 **cls zhen mei**；如果是其他，则无输出

```shell
[root@192 ShellScripts]# touch if.sh
[root@192 ShellScripts]# vim if.sh

# 脚本内容如下
#!/bin/bash
if [ $1 -eq 1 ];then
	echo 'banzhang zhen shuai'
elif [ $1 -eq 2 ];then
	echo 'cls zhen mei'
fi

[root@192 ShellScripts]# chmod 777 if.sh
[root@192 ShellScripts]# ./if.sh 1
```



### 2.case语句

#### 基本语法

```shell
case $变量名 in

 "val1")
	如果变量的值等于val1，则执行程序1
	;;
 "val2")
 	如果变量的值等于val2，则执行程序2
 	;;
 ...省略其他分支...
 *)
 	如果变量的值都不是以上的值，则执行此程序
 	;;
esac
```

==注意事项：==

（1）case 行尾必须为单词“in”，每一个模式匹配必须以“)”结束

（2）双分号“;;”表示命令序列结束，相当于 java 中的 break

（3）最后的“*)”表示默认模式，相当于 java 中的 default



#### 实操案例

输入一个数字，如果是**1**，则输出 **banzhang**；如果是 **2**，则输出 **cls**；如果是其它，输出 **renyao**

```shell
[root@192 ShellScripts]# touch case.sh
[root@192 ShellScripts]# vim case.sh

# 脚本内容如下
#!/bin/bash
case $1 in
 1)
   echo banzhang
   ;;
 2)
   echo cls
   ;;
 *)
   echo renyao
   ;;
esac

[root@192 ShellScripts]# bash case.sh
```



### 3.for循环

#### 基本语法

```shell
for((初始值;循环控制条件;变量变化))
  do
    程序
  done
```

或

```shell
for 变量 in val1 val2 val3...
  do
    程序
  done
```



#### 实操案例

从1加到100

```shell
[root@192 ShellScripts]# touch for.sh
[root@192 ShellScripts]# vim for.sh

# 脚本内容如下
#!/bin/bash
s=0
for((i=0;i<=100;i++))
  do
    s=$[$s+$i]
  done
echo $s

[root@192 ShellScripts]# bash for.sh
```



打印所有输入的参数，使用**$@**

```shell
[root@192 ShellScripts]# touch for2.sh
[root@192 ShellScripts]# vim for2.sh

# 脚本内容如下
#!/bin/bash
for i in $*
  do
    echo $i
  done

for j in $@
  do
    echo $j
  done

for k in "$*"
  do
    echo $k
  done

for l in "$@"
  do
    echo $l
  done

[root@192 ShellScripts]# bash for2.sh
```



### 4.while循环

#### 基本语法

```shell
while [ 条件判断式 ]
  do
    程序
  done
```



#### 实操案例

从1加到100

```shell
[root@192 ShellScripts]# touch while.sh
[root@192 ShellScripts]# vim while.sh

# 脚本内容如下
#!/bin/bash
s=0
i=1
while [ $i -le 100 ]
  do
    s=$[$s+$i]
    i=$[$i+1]
  done
echo $s

[root@192 ShellScripts]# bash while.sh
```



## read读取控制台输入

### 基本语法

TODO
