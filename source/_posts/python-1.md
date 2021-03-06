title: 初学Python o(∩_∩)o 、
url: 3329.html
id: 3329
categories:
  - Python
date: 2016-02-08 19:16:20
tags:
---
Python练习一波
<!--more-->
### 练习之前应该知道的

*   Linux命令行将以 \\$ 开始，比如 \\$ls, $python
*   Python命令行将以 >>> 开始，比如 >>>print 'Hello World!'
*   注释会以 # 开始

### 一.Shell界面输入python可以进入python执行状态

##### ![mm](http://blog.istarboy.cc/wp-content/uploads/2016/02/mm-300x51.png)

然后可以执行相关内容。

### 二.>>>print('Hello World!')

print是一个常用函数，其功能就是输出括号中得字符串。 直接在python解释器状态运行会输出Hello World! 也可以写入py文件使用python *.py运行程序，在py文件头加上 #!/usr/bin/env python  可以直接进行chmod +x *.py ./*.py执行测试脚本。

### 三.变量

Python的变量不需要声明，你可以直接输入：a = 10 那么内存里就有了一个变量a， 它的值是10，它的类型是integer (整数)。无需声明，数据类型是Python自动决定的。print type(a)可以显示a变量的数据类型。

### 四.数据类型

变量

数据类型

a=10

int 整数

a=1.3

float 浮点数

a=True

真值(True/False)

a='Hello!'

字符串

### 五.序列

sequence(序列)是一组**有顺序**的**元素**的**集合** (严格的说，是对象的集合，但鉴于我们还没有引入“对象”概念，暂时说元素) 序列可以包含一个或多个**元素**，也可以没有任何元素。 我们之前所说的基本数据类型，都可以作为序列的元素。元素还可以是另一个序列，以及我们以后要介绍的其他对象。 序列有两种：tuple（**定值表**； 也有翻译为**元组**） 和 list (**表**)

    >>>s1 = (2, 1.3, 'love', 5.6, 9, 12, False)         # s1是一个tuple
    >>>s2 = [True, 5, 'smile']                          # s2是一个list
    >>>print s1,type(s1)
    >>>print s2,type(s2)
    

tuple和list的主要区别在于，一旦建立，**tuple的各个元素不可再变更，而list的各个元素可以再变更**。 一个序列作为另一个序列的元素：

    >>>s3 = [1,[3,4,5]]
    

空序列：

    >>>s4 = []
    

### 1、元素的引用

序列元素的下标从0开始：

    >>>print s1[0]
    >>>print s2[2]
    >>>print s3[1][2]
    

由于list的元素可变更，你可以对list的某个元素赋值：

    >>>s2[1] = 3.0
    >>>print s2
    

如果你对tuple做这样的操作，会得到错误提示。 所以，可以看到，序列的引用通过s\[int\]实现，(int为下标)。

### 2、其他引用方式

范围引用： 基本样式 **\[下限:上限:步长\]**

    >>>print s1[:5]             # 从开始到下标4 （下标5的元素 不包括在内）
    >>>print s1[2:]             # 从下标2到最后
    >>>print s1[0:5:2]          # 从下标0到下标4 (下标5不包括在内)，每隔2取一个元素 （下标为0，2，4的元素）
    >>>print s1[2:0:-1]         # 从下标2到下标1
    

从上面可以看到，在范围引用的时候，如果写明上限，那么**这个上限本身不包括在内**。 尾部元素引用：

    >>>print s1[-1]             # 序列最后一个元素
    >>>print s1[-3]             # 序列倒数第三个元素
    

同样，如果s1\[0:-1\], 那么最后一个元素不会被引用 （再一次，**不包括上限元素本身**）。

### 3、字符串是元组

字符串是一种特殊的元素，因此可以执行元组的相关操作。

    >>>str = 'abcdef'
    >>>print str[2:4]
    

六、运算
----

### 1、数学运算

    >>>print 1+9        # 加法
    >>>print 1.3-4      # 减法
    >>>print 3*5        # 乘法
    >>>print 4.5/1.5    # 除法
    >>>print 3**2       # 乘方     
    >>>print 10%3       # 求余数
    

### 2、判断

判断是真还是假，返回True/False:

    >>>print 5==6               # =， 相等
    >>>print 8.0!=8.0           # !=, 不等
    >>>print 3<3, 3<=3          # <, 小于; <=, 小于等于
    >>>print 4>5, 4>=0          # >, 大于; >=, 大于等于
    >>>print 5 in [1,3,5]       # 5是list [1,3,5]的一个元素
    

> 还有is, is not等, 暂时不深入。

### 3、逻辑运算

True/False之间的运算：

    >>>print True and True, True and False      # and, “与”运算， 两者都为真才是真
    >>>print True or False                      # or, "或"运算， 其中之一为真即为真
    >>>print not True                           # not, “非”运算， 取反
    

可以和上一部分结合做一些练习，比如：

    >>>print 5==6 or 3>=3
    

七、缩进和选择
-------

### 1、缩进

Python最具特色的是用缩进来标明成块的代码。我下面以if选择结构来举例。if后面跟随条件，如果条件成立，则执行归属于 if 的一个代码块。 先看C语言的表达方式（注意，**这是C，不是Python!**）

    if ( i > 0 )
    {
        x = 1;
        y = 2;
    }
    

如果i > 0的话，我们将进行括号中所包括的两个赋值操作。括号中包含的就是块操作，它隶属于if。 在Python中，同样的目的，这段话是这样的：

    if i > 0:
        x = 1
        y = 2
    

在Python中， 去掉了i > 0周围的括号，去除了每个语句句尾的分号，表示块的花括号也消失了。 多出来了if ...之后的 **:(冒号)**, 还有就是x = 1 和 y =2前面有**四个空格的缩进**。通过缩进，Python识别出这两个语句是隶属于if。 Python这样设计的理由纯粹是为了程序好看。

### 2、if语句

写一个完整的程序，命名为ifDemo.py。这个程序用于实现if结构。

    i = 1
    x = 1
    if i > 0:
        x = x+1
    print x
    

用cd命令进入该文件所在目录，然后输入命令运行它：

    $python ifDemo.py  # 运行
    

程序运行到 if 的时候，条件为True，因此执行**x = x+1**。 **print x**语句没有缩进，那么就是if之外。 如果将第一句改成i = -1，那么 if 遇到假值 (False), **x = x+1**隶属于 if , 这一句跳过。**print x**没有缩进，是 if 之外，不跳过，继续执行。 这种以**四个空格**的缩进来表示**隶属关系**的书写方式，以后还会看到。强制缩进增强了程序的**可读性**。 复杂一些的 if 选择：

    i = 1
    if i > 0:
        print 'positive i'
        i = i + 1
    elif i == 0:
        print 'i is 0'
        i = i * 10
    else:
        print 'negative i'
        i = i - 1
    print 'new i:',i
    

这里有三个块，分别属于**if，elif，else**引领。 Python检测条件，如果发现 if 的条件为假，那么跳过后面紧跟的块，检测下一个 elif 的条件； 如果还是假，那么执行else块。 通过上面的结构将程序分出三个分支。程序根据条件，只执行三个分支中的一个。 整个 if 可以放在另一个 if 语句中，也就是 if 结构的嵌套使用：

    i  = 5
    if i > 1:
        print 'i bigger than 1'
        print 'good'
        if i > 2:
            print 'i bigger than 2'
            print 'even better'
    

if i > 2 后面的块相对于该 if 缩进了四个空格，以表明其隶属于该 if ，而不是外层的 if 。