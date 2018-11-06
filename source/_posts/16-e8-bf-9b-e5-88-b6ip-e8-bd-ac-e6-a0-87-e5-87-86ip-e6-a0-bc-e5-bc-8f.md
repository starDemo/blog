---
title: 16进制IP转标准IP格式
url: 3377.html
id: 3377
categories:
  - Codeing
date: 2016-04-26 00:22:51
tags:
---

移位或者用socket函数

C/C++ code

[?](http://bbs.csdn.net/topics/310119169#clipboardWindow "源码")

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

`void` `IPToString(``DWORD` `dwIP, ``char` `csIP[16]) `

`{`

`ZeroMemory(csIP, 16);`

`WORD` `add1,add2,add3,add4;`

`add1 = (``WORD``)(dwIP&255);`

`add2 = (``WORD``)((dwIP>>8)&255);`

`add3 = (``WORD``)((dwIP>>16)&255);`

`add4 = (``WORD``)((dwIP>>24)&255);`

`sprintf``(csIP, ``"%d.%d.%d.%d"``, add4, add3, add2, add1);`

`}`

`char``* ValueToString(``DWORD` `dwValue)   `

`{`

`in_addr  ipAddr;`

`ipAddr.S_un.S_addr = dwValue;`

`return` `inet_ntoa(ipAddr);`

`}`

`如``printf``(``"%s"``, ValueToString(3232235769);`

`输出为192,168,0,249`