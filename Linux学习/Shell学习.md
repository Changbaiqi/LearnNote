---
title: Shell学习
date: 2023-02-24 09:46:09
author: 长白崎
categories:
  - "Linux"
tags:
  - ["Linux","shell"]
---



# Shell学习

---

### echo

说明：#! 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。echo 命令用于向窗口输出文本。

```shell
#!/bin/bash
echo "Hello World!"
```







## Shell 变量

在 Shell 编程中，变量是用于存储数据值的名称。

定义变量时，变量名不加美元符号（$，PHP语言中变量需要），如：

```shell
shellyour_name="runoob"
```



注意，变量名和等号之间不能有空格，这可能和你熟悉的所有编程语言都不一样。同时，变量名的命名须遵循如下规则：

- 只包含字母、数字和下划线： 变量名可以包含字母（大小写敏感）、数字和下划线 _，不能包含其他特殊字符。
- 不能以数字开头： 变量名不能以数字开头，但可以包含数字。
- 避免使用 Shell 关键字： 不要使用Shell的关键字（例如 if、then、else、fi、for、while 等）作为变量名，以免引起混淆。
- 使用大写字母表示常量： 习惯上，常量的变量名通常使用大写字母，例如 PI=3.14。
- 避免使用特殊符号： 尽量避免在变量名中使用特殊符号，因为它们可能与 Shell 的语法产生冲突。
- 避免使用空格： 变量名中不应该包含空格，因为空格通常用于分隔命令和参数。

有效的 Shell 变量名示例如下：

```shell
RUNOOB="www.runoob.com"
LD_LIBRARY_PATH="/bin/"
_var="123"
var2="abc"
```

无效的变量命名：

```shell
# 避免使用if作为变量名
if="some_value"
# 避免使用 $ 等特殊符号
variable_with_$=42
?var=123
user*name=runoob
# 避免空格
variable with space="value"
```

等号两侧避免使用空格：

```shell
# 正确的赋值
variable_name=value

# 有可能会导致错误
variable_name = value
```

除了显式地直接赋值，还可以用语句给变量赋值，如：

```shell
for file in `ls /etc`
或
for file in $(ls /etc)
```

以上语句将`/etc`下目录的文件名循环出来。

### 使用变量

使用一个定义过的变量，只要在变量名前面加美元符号即可，如：

```shell
your_name="qinjx"
echo $your_name
echo ${your_name}
```

变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况：

```shell
for skill in Ada Coffe Action Java; do
	echo "I am good at ${skill}Script"
done
```

如果不给skill变量加花括号，写成`echo "I am good at $skillScript"`，解释器就会把$skillScript当成一个变量（其值为空），代码执行结果就不是我们期望的样子了。

推荐给所有变量加上花括号，这是个好的编程习惯。

已定义的变量，可以被重新定义，如：

```shell
your_name="tom"
echo $your_name
your_name="alibaba"
echo $your_name
```

这样写法是合法的，但注意，第二次赋值的时候不能写$your_name="alibaba",使用变量的时候才加美元符(\$)。

### 只读变量

使用 readonly 命令可以将变量定义为只读变量，只读变量的值不能被改变。

下面的例子尝试更改只读变量，结果报错：

```shell
#!/bin/bash
myUrl="https://www.google.com"
readonly myUrl
myUrl="https://www.runoob.com"
```

运行脚本，结果如下：

```shell
/bin/sh: NAME: This variable is read only.
```

### 删除变量

使用 unset 命令可以删除变量。语法：

```shell
unset variable_name
```

变量被删除后不能再次使用。unset 命令不能删除只读变量。

```shell
#!/bin/sh
myUrl="https://www.runoob.com"
unset myUrl
echo $myUrl
```

以上实例执行将没有任何输出。

### 变量类型

Shell 支持不同类型的变量，其中一些主要的类型包括：

字符串变量： 在 Shell中，变量通常被视为字符串。

你可以使用单引号 `'` 或双引号 `"` 来定义字符串，例如：

```shell
my_string='Hello, World!'
或者
my_string="Hello, World!"
```

整数变量： 在一些Shell中，你可以使用 declare 或 typeset 命令来声明整数变量。

这样的变量只包含整数值，例如：

```shell
declare -i my_integer=42
```

这样的声明告诉 Shell 将 my_integer 视为整数，如果尝试将非整数值赋给它，Shell会尝试将其转换为整数。

数组变量： Shell 也支持数组，允许你在一个变量中存储多个值。

数组可以是整数索引数组或关联数组，以下是一个简单的整数索引数组的例子：

```shell
my_array=(1 2 3 4 5)
```

或者关联数组：

```shell
declare -A associative_array
associative_array["name"]="John"
associative_array["age"]=30
```

环境变量： 这些是由操作系统或用户设置的特殊变量，用于配置 Shell 的行为和影响其执行环境。

例如，PATH 变量包含了操作系统搜索可执行文件的路径：

```
echo $PATH
```

特殊变量： 有一些特殊变量在 Shell 中具有特殊含义，例如 $0 表示脚本的名称，$1, $2, 等表示脚本的参数。

$#表示传递给脚本的参数数量，$? 表示上一个命令的退出状态等。

------









## Shell 传递参数

我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为 $n，n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数。

例如可以使用 $1、$2 等来引用传递给脚本的参数，其中 $1 表示第一个参数，$2 表示第二个参数，依此类推。

### 实例

以下实例我们向脚本传递三个参数，并分别输出，其中 $0 为执行的文件名（包含文件路径）：

```shell
#!/bin/bash
# author:长白崎
# url:www.changbaiqi.top
echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

为脚本设置可执行权限，并执行脚本，输出结果如下所示：

```shell
$ chmod +x test.sh 
$ ./test.sh 1 2 3
Shell 传递参数实例！
执行的文件名：./test.sh
第一个参数为：1
第二个参数为：2
第三个参数为：3
```

另外，还有几个特殊字符用来处理参数：

| 参数处理 | 说明                                                         |
| :------- | :----------------------------------------------------------- |
| $#       | 传递到脚本的参数个数                                         |
| $*       | 以一个单字符串显示所有向脚本传递的参数。 如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。 |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程的ID号                                 |
| $@       | 与$*相同，但是使用时加引号，并在引号中返回每个参数。 如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。 |
| $-       | 显示Shell使用的当前选项，与[set命令](https://www.runoob.com/linux/linux-comm-set.html)功能相同。 |
| $?       | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |

### 实例

```shell
#!/bin/bash*
# author:长白崎
# url:www.changbaiqi.top
echo "Shell 传递参数实例！";
echo "第一个参数为：$1";
echo "参数个数为：$#";
echo "传递的参数作为一个字符串显示：$*";


```

执行脚本，输出结果如下所示：

```shell
$ chmod +x test.sh 
$ ./test.sh 1 2 3
Shell 传递参数实例！
第一个参数为：1
参数个数为：3
传递的参数作为一个字符串显示：1 2 3
```

`$*` 与 `$@` 区别：

- 相同点：都是引用所有参数。
- 不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。

### 实例

```shell
#!/bin/bash*
# author:菜鸟教程*
# url:www.runoob.com*

echo "-- \$* 演示 ---"
for i in "$*"; do
  echo $i
done

echo "-- \$@ 演示 ---"
for i in "$@"; do
  echo $i
done
```

执行脚本，输出结果如下所示：

```shell
$ chmod +x test.sh 
$ ./test.sh 1 2 3
-- $* 演示 ---
1 2 3
-- $@ 演示 ---
1
2
3
```





## Shell 基本运算符

Shell 和其他编程语言一样，支持多种运算符，包括：

- 算数运算符
- 关系运算符
- 布尔运算符
- 字符串运算符
- 文件测试运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。

expr 是一款表达式计算工具，使用它能完成表达式的求值操作。

例如，两个数相加(注意使用的是反引号 ` 而不是单引号 ' )：

### 实例

```shell
#!/bin/bash*
val=`expr 2 + 2`
echo "两数之和为 : $val"
```

执行脚本，输出结果如下所示：

```shell
两数之和为 : 4
```

两点注意：

- 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样。
- 完整的表达式要被 ` ` 包含，注意这个字符不是常用的单引号，在 Esc 键下边。

------

### 算术运算符

下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                          | 举例                          |
| :----- | :-------------------------------------------- | :---------------------------- |
| +      | 加法                                          | `expr $a + $b` 结果为 30。    |
| -      | 减法                                          | `expr $a - $b` 结果为 -10。   |
| *      | 乘法                                          | `expr $a \* $b` 结果为  200。 |
| /      | 除法                                          | `expr $b / $a` 结果为 2。     |
| %      | 取余                                          | `expr $b % $a` 结果为 0。     |
| =      | 赋值                                          | a=$b 把变量 b 的值赋给 a。    |
| ==     | 相等。用于比较两个数字，相同则返回 true。     | [ $a == $b ] 返回 false。     |
| !=     | 不相等。用于比较两个数字，不相同则返回 true。 | [ $a != $b ] 返回 true。      |

注意：条件表达式要放在方括号之间，并且要有空格，例如: [$a==$b] 是错误的，必须写成 [ $a == $b ]。

#### 实例

算术运算符实例如下：

```shell
#!/bin/bash

a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $b / $a`
echo "b / a : $val"

val=`expr $b % $a`
echo "b % a : $val"

if [ $a == $b ]
then
  echo "a 等于 b"
fi
if [ $a != $b ]
then
  echo "a 不等于 b"
fi
```



执行脚本，输出结果如下所示：

```shell
a + b : 30
a - b : -10
a * b : 200
b / a : 2
b % a : 0
a 不等于 b
```

> 注意：
>
> - 乘号(*)前边必须加反斜杠(\)才能实现乘法运算；
> - if...then...fi 是条件语句，后续将会讲解。
> - 在 MAC 中 shell 的 expr 语法是：$((表达式))，此处表达式中的 "*" 不需要转义符号 "\\" 。

------

### 关系运算符

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                                  | 举例                         |
| :----- | :---------------------------------------------------- | :--------------------------- |
| -eq    | 检测两个数是否相等，相等返回 true。                   | [ \$a -eq \$b ] 返回 false。 |
| -ne    | 检测两个数是否不相等，不相等返回 true。               | [ \$a -ne \$b ] 返回 true。  |
| -gt    | 检测左边的数是否大于右边的，如果是，则返回 true。     | [ \$a -gt \$b ] 返回 false。 |
| -lt    | 检测左边的数是否小于右边的，如果是，则返回 true。     | [ \$a -lt \$b ] 返回 true。  |
| -ge    | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | [ \$a -ge \$b ] 返回 false。 |
| -le    | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [ \$a -le \$b ] 返回 true。  |

#### 实例

关系运算符实例如下：

```shell
#!/bin/bash
a=10
b=20

if [ $a -eq $b ]
then
  echo "$a -eq $b : a 等于 b"
else
  echo "$a -eq $b: a 不等于 b"
fi
if [ $a -ne $b ]
then
  echo "$a -ne $b: a 不等于 b"
else
  echo "$a -ne $b : a 等于 b"
fi
if [ $a -gt $b ]
then
  echo "$a -gt $b: a 大于 b"
else
  echo "$a -gt $b: a 不大于 b"
fi
if [ $a -lt $b ]
then
  echo "$a -lt $b: a 小于 b"
else
  echo "$a -lt $b: a 不小于 b"
fi
if [ $a -ge $b ]
then
  echo "$a -ge $b: a 大于或等于 b"
else
  echo "$a -ge $b: a 小于 b"
fi
if [ $a -le $b ]
then
  echo "$a -le $b: a 小于或等于 b"
else
  echo "$a -le $b: a 大于 b"
fi
```

执行脚本，输出结果如下所示：

```shell
10 -eq 20: a 不等于 b
10 -ne 20: a 不等于 b
10 -gt 20: a 不大于 b
10 -lt 20: a 小于 b
10 -ge 20: a 小于 b
10 -le 20: a 小于或等于 b
```

------

### 布尔运算符

下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                                | 举例                                       |
| :----- | :-------------------------------------------------- | :----------------------------------------- |
| !      | 非运算，表达式为 true 则返回 false，否则返回 true。 | [ ! false ] 返回 true。                    |
| -o     | 或运算，有一个表达式为 true 则返回 true。           | [ \$a -lt 20 -o \$b -gt 100 ] 返回 true。  |
| -a     | 与运算，两个表达式都为 true 才返回 true。           | [ \$a -lt 20 -a \$b -gt 100 ] 返回 false。 |

#### 实例

布尔运算符实例如下：

```shell
#!/bin/bash

a=10
b=20

if [ $a != $b ]
then
  echo "$a != $b : a 不等于 b"
else
  echo "$a == $b: a 等于 b"
fi
if [ $a -lt 100 -a $b -gt 15 ]
then
  echo "$a 小于 100 且 $b 大于 15 : 返回 true"
else
  echo "$a 小于 100 且 $b 大于 15 : 返回 false"
fi
if [ $a -lt 100 -o $b -gt 100 ]
then
  echo "$a 小于 100 或 $b 大于 100 : 返回 true"
else
  echo "$a 小于 100 或 $b 大于 100 : 返回 false"
fi
if [ $a -lt 5 -o $b -gt 100 ]
then
  echo "$a 小于 5 或 $b 大于 100 : 返回 true"
else
  echo "$a 小于 5 或 $b 大于 100 : 返回 false"
fi
```



执行脚本，输出结果如下所示：

```
10 != 20 : a 不等于 b
10 小于 100 且 20 大于 15 : 返回 true
10 小于 100 或 20 大于 100 : 返回 true
10 小于 5 或 20 大于 100 : 返回 false
```

------

### 逻辑运算符

以下介绍 Shell 的逻辑运算符，假定变量 a 为 10，变量 b 为 20:

| 运算符 | 说明       | 举例                                         |
| :----- | :--------- | :------------------------------------------- |
| &&     | 逻辑的 AND | [[ \$a -lt 100 && \$b -gt 100 ]] 返回 false  |
| \|\|   | 逻辑的 OR  | [[ \$a -lt 100 \|\| \$b -gt 100 ]] 返回 true |

#### 实例

逻辑运算符实例如下：

```shell
#!/bin/bash
a=10
b=20

if [[ $a -lt 100 && $b -gt 100 ]]
then
  echo "返回 true"
else
  echo "返回 false"
fi

if [[ $a -lt 100 || $b -gt 100 ]]
then
  echo "返回 true"
else
  echo "返回 false"
fi
```



执行脚本，输出结果如下所示：

```
返回 false
返回 true
```

------

### 字符串运算符

下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：

| 运算符 | 说明                                         | 举例                       |
| :----- | :------------------------------------------- | :------------------------- |
| =      | 检测两个字符串是否相等，相等返回 true。      | [ \$a = \$b ] 返回 false。 |
| !=     | 检测两个字符串是否不相等，不相等返回 true。  | [ \$a != \$b ] 返回 true。 |
| -z     | 检测字符串长度是否为0，为0返回 true。        | [ -z $a ] 返回 false。     |
| -n     | 检测字符串长度是否不为 0，不为 0 返回 true。 | [ -n "$a" ] 返回 true。    |
| $      | 检测字符串是否不为空，不为空返回 true。      | [ $a ] 返回 true。         |

### 实例

字符串运算符实例如下：

```shell
#!/bin/bash

a="abc"
b="efg"

if [ $a = $b ]
then
  echo "$a = $b : a 等于 b"
else
  echo "$a = $b: a 不等于 b"
fi
if [ $a != $b ]
then
  echo "$a != $b : a 不等于 b"
else
  echo "$a != $b: a 等于 b"
fi
if [ -z $a ]
then
  echo "-z $a : 字符串长度为 0"
else
  echo "-z $a : 字符串长度不为 0"
fi
if [ -n "$a" ]
then
  echo "-n $a : 字符串长度不为 0"
else
  echo "-n $a : 字符串长度为 0"
fi
if [ $a ]
then
  echo "$a : 字符串不为空"
else
  echo "$a : 字符串为空"
fi
```





执行脚本，输出结果如下所示：

```shell
abc = efg: a 不等于 b
abc != efg : a 不等于 b
-z abc : 字符串长度不为 0
-n abc : 字符串长度不为 0
abc : 字符串不为空
```

------

### 文件测试运算符

文件测试运算符用于检测 Unix 文件的各种属性。

属性检测描述如下：

| 操作符  | 说明                                                         | 举例                      |
| :------ | :----------------------------------------------------------- | :------------------------ |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true。              | [ -b $file ] 返回 false。 |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true。            | [ -c $file ] 返回 false。 |
| -d file | 检测文件是否是目录，如果是，则返回 true。                    | [ -d $file ] 返回 false。 |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 | [ -f $file ] 返回 true。  |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。            | [ -g $file ] 返回 false。 |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。  | [ -k $file ] 返回 false。 |
| -p file | 检测文件是否是有名管道，如果是，则返回 true。                | [ -p $file ] 返回 false。 |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。            | [ -u $file ] 返回 false。 |
| -r file | 检测文件是否可读，如果是，则返回 true。                      | [ -r $file ] 返回 true。  |
| -w file | 检测文件是否可写，如果是，则返回 true。                      | [ -w $file ] 返回 true。  |
| -x file | 检测文件是否可执行，如果是，则返回 true。                    | [ -x $file ] 返回 true。  |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true。     | [ -s $file ] 返回 true。  |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。          | [ -e $file ] 返回 true。  |

其他检查符：

- -S: 判断某文件是否 socket。
- -L: 检测文件是否存在并且是一个符号链接。



#### 实例

变量 file 表示文件 /var/www/runoob/test.sh，它的大小为 100 字节，具有 rwx 权限。下面的代码，将检测该文件的各种属性：

```shell
#!/bin/bash

file="/var/www/runoob/test.sh"
if [ -r $file ]
then
  echo "文件可读"
else
  echo "文件不可读"
fi
if [ -w $file ]
then
  echo "文件可写"
else
  echo "文件不可写"
fi
if [ -x $file ]
then
  echo "文件可执行"
else
  echo "文件不可执行"
fi
if [ -f $file ]
then
  echo "文件为普通文件"
else
  echo "文件为特殊文件"
fi
if [ -d $file ]
then
  echo "文件是个目录"
else
  echo "文件不是个目录"
fi
if [ -s $file ]
then
  echo "文件不为空"
else
  echo "文件为空"
fi
if [ -e $file ]
then
  echo "文件存在"
else
  echo "文件不存在"
fi
```



执行脚本，输出结果如下所示：

```shell
文件可读
文件可写
文件可执行
文件为普通文件
文件不是个目录
文件不为空
文件存在
```

### 自增和自减操作符

尽管 Shell 本身没有像 C、C++ 或 Java 那样的 ++ 和 -- 操作符，但可以通过其他方式实现相同的功能。以下是一些常见的方法：

### 使用 let 命令

let 命令允许对整数进行算术运算。

```shell
#!/bin/bash

# 初始化变量
num=5

# 自增
let num++

# 自减
let num--

echo $num
```



### 使用 $(( )) 进行算术运算

`$(( ))` 语法也是进行算术运算的一种方式。

#### 实例

```shell
#!/bin/bash

# 初始化变量
num=5

# 自增
num=$((num + 1))

# 自减
num=$((num - 1))

echo $num
```

### 使用 expr 命令

expr 命令可以用于算术运算，但在现代脚本中不如 let 和 $(( )) 常用。

#### 实例

```shell
#!/bin/bash
# 初始化变量
num=5

# 自增
num=$(expr $num + 1)

# 自减
num=$(expr $num - 1)

echo $num
```



### 使用 (( )) 进行算术运算

与 $(( )) 类似，(( )) 语法也可以用于算术运算。

#### 实例

```shell
#!/bin/bash

# 初始化变量
num=5

# 自增
((num++))

# 自减
((num--))

echo $num
```



#### 实例

以下是一个完整的示例脚本，演示了自增和自减操作的使用：

```shell
#!/bin/bash

# 初始化变量
num=5

echo "初始值: $num"

# 自增
let num++
echo "自增后: $num"

# 自减
let num--
echo "自减后: $num"

# 使用 $(( ))
num=$((num + 1))
echo "使用 $(( )) 自增后: $num"

num=$((num - 1))
echo "使用 $(( )) 自减后: $num"

# 使用 expr
num=$(expr $num + 1)
echo "使用 expr 自增后: $num"

num=$(expr $num - 1)
echo "使用 expr 自减后: $num"

# 使用 (( ))
((num++))
echo "使用 (( )) 自增后: $num"

((num--))
echo "使用 (( )) 自减后: $num"
```



运行这个脚本会输出以下内容：

```shell
初始值: 5
自增后: 6
自减后: 5
使用 $(( )) 自增后: 6
使用 $(( )) 自减后: 5
使用 expr 自增后: 6
使用 expr 自减后: 5
使用 (( )) 自增后: 6
使用 (( )) 自减后: 5
```



## Shell 函数

---

linux shell 可以用户定义函数，然后在shell脚本中可以随便调用。

shell中函数的定义格式如下：

```shell
[ function ] funname [()]
{

  action;

  [return int;]

}
```



说明：

- 1、可以带 function fun() 定义，也可以直接 fun() 定义,不带任何参数。
- 2、参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return 后跟数值 n(0-255).

下面的例子定义了一个函数并进行调用：

#### 实例

```shell
#!/bin/bash

demoFun(){
  echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"
```



输出结果：

```
-----函数开始执行-----
这是我的第一个 shell 函数!
-----函数执行完毕-----
```

下面定义一个带有 return 语句的函数：

#### 实例

```shell
#!/bin/bash

funWithReturn(){
  echo "这个函数会对输入的两个数字进行相加运算..."
  echo "输入第一个数字: "
  read aNum
  echo "输入第二个数字: "
  read anotherNum
  echo "两个数字分别为 $aNum 和 $anotherNum !"
  return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```



输出类似下面：

```shell
这个函数会对输入的两个数字进行相加运算...
输入第一个数字: 
1
输入第二个数字: 
2
两个数字分别为 1 和 2 !
输入的两个数字之和为 3 !
```

函数返回值在调用该函数后通过 $? 来获得。

注意：所有函数在使用前必须定义。这意味着必须将函数放在脚本开始部分，直至shell解释器首次发现它时，才可以使用。调用函数仅使用其函数名即可。

注意： return 语句只能返回一个介于 0 到 255 之间的整数，而两个输入数字的和可能超过这个范围。

要解决这个问题，您可以修改 return 语句，直接使用 echo 输出和而不是使用 return：

#### 实例

```shell
funWithReturn(){
  echo "这个函数会对输入的两个数字进行相加运算..."
  echo "输入第一个数字: "
  read aNum
  echo "输入第二个数字: "
  read anotherNum
  sum=$(($aNum + $anotherNum))
  echo "两个数字分别为 $aNum 和 $anotherNum !"
  echo $sum # 输出两个数字的和
}
```



### 函数参数

在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 \$n 的形式来获取参数的值，例如，\$1表示第一个参数，\$2表示第二个参数...

带参数的函数示例：

#### 实例

```shell
#!/bin/bash

funWithParam(){
  echo "第一个参数为 $1 !"
  echo "第二个参数为 $2 !"
  echo "第十个参数为 $10 !"
  echo "第十个参数为 ${10} !"
  echo "第十一个参数为 ${11} !"
  echo "参数总数有 $# 个!"
  echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```



输出结果：

```shell
第一个参数为 1 !
第二个参数为 2 !
第十个参数为 10 !
第十个参数为 34 !
第十一个参数为 73 !
参数总数有 11 个!
作为一个字符串输出所有参数 1 2 3 4 5 6 7 8 9 34 73 !
```

注意，$10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。

另外，还有几个特殊字符用来处理参数：

| 参数处理 | 说明                                                         |
| :------- | :----------------------------------------------------------- |
| $#       | 传递到脚本或函数的参数个数                                   |
| $*       | 以一个单字符串显示所有向脚本传递的参数                       |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程的ID号                                 |
| $@       | 与$*相同，但是使用时加引号，并在引号中返回每个参数。         |
| $-       | 显示Shell使用的当前选项，与set命令功能相同。                 |
| $?       | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |