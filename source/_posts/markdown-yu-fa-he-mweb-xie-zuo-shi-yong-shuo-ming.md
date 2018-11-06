---
title: Markdown 语法和 MWeb 写作使用说明
tags:
  - MWeb
url: 3564.html
id: 3564
categories:
  - 杂侃
date: 2018-01-06 16:24:05
---

Markdown 的设计哲学
--------------

> Markdown 的目標是實現「易讀易寫」。  
> 不過最需要強調的便是它的可讀性。一份使用 Markdown 格式撰寫的文件應該可以直接以純文字發佈，並且看起來不會像是由許多標籤或是格式指令所構成。  
> Markdown 的語法有個主要的目的：用來作為一種網路內容的_寫作_用語言。

本文约定
----

如果有写 `效果如下：`， 在 MWeb 编辑状态下只有用 `CMD + 4` 或 `CMD + R` 预览才可以看效果。

标题
--

Markdown 语法：

    # 第一级标题 `<h1>`
    ## 第二级标题 `<h2>`
    ###### 第六级标题 `<h6>`
    

效果如下：

第一级标题 `<h1>`
============

第二级标题 `<h2>`
------------

###### 第六级标题 `<h6>`

强调
--

Markdown 语法：

    *这些文字会生成`<em>`*
    _这些文字会生成`<u>`_
    **这些文字会生成`<strong>`**
    __这些文字会生成`<strong>`__
    

在 MWeb 中的快捷键为： `CMD + U`、`CMD + I`、`CMD + B`  
效果如下：

_这些文字会生成`<em>`_  
这些文字会生成`<u>`

**这些文字会生成`<strong>`**  
**这些文字会生成`<strong>`**

换行
--

四个及以上空格加回车。  
如果不想打这么多空格，只要回车就为换行，请勾选：`Preferences` \- `Themes` \- `Translate newlines to <br> tags`

列表
--

### 无序列表

Markdown 语法：

    * 项目一 无序列表 `* + 空格键`
    * 项目二
        * 项目二的子项目一 无序列表 `TAB + * + 空格键`
        * 项目二的子项目二
    

在 MWeb 中的快捷键为： `Option + U`  
效果如下：

*   项目一 无序列表 `* + 空格键`
*   项目二
    *   项目二的子项目一 无序列表 `TAB + * + 空格键`
    *   项目二的子项目二

### 有序列表

Markdown 语法：

    1. 项目一 有序列表 `数字 + . + 空格键`
    2. 项目二
    3. 项目三
        1. 项目三的子项目一 有序列表 `TAB + 数字 + . + 空格键`
        2. 项目三的子项目二
    

效果如下：

1.  项目一 有序列表 `数字 + . + 空格键`
2.  项目二
3.  项目三
    1.  项目三的子项目一 有序列表 `TAB + 数字 + . + 空格键`
    2.  项目三的子项目二

### 列表中嵌入代码块语法

    1. 项目一 有序列表 `数字 + . + 空格键`
        列表中嵌入代码块必须前后空一行，如这个写法
        ```js
        function fancyAlert(arg) {
          if(arg) {
            $.facebox({div:'#foo'})
          }
        }
        ```
        其他文本。
    2. 项目二
    

### 任务列表（Task lists）

Markdown 语法：

    - [ ] 任务一 未做任务 `- + 空格 + [ ]`
    - [x] 任务二 已做任务 `- + 空格 + [x]`
    

效果如下：

*    任务一 未做任务 `- + 空格 + [ ]`
*    任务二 已做任务 `- + 空格 + [x]`

图片
--

Markdown 语法：

    ![GitHub set up](http://zh.mweb.im/asset/img/set-up-git.gif)
    格式: ![Alt Text](url)
    

`Control + Shift + I` 可插入Markdown语法。  
如果是 MWeb 的文档库中的文档，还可以用拖放图片、`CMD + V` 粘贴、`CMD + Option + I` 导入这三种方式来增加图片。  
效果如下：

![GitHub set up](http://zh.mweb.im/asset/img/set-up-git.gif)

MWeb 引入的特别的语法来设置图片宽度，方法是在图片描述后加 `-w + 图片宽度` 即可，比如说要设置上面的图片的宽度为 140，语法如为 `![GitHub-w140](set-up-git.gif)`：

![GitHub set up](http://zh.mweb.im/asset/img/set-up-git.gif)

链接
--

Markdown 语法：

    email <example@example.com>
    [GitHub](http://github.com)
    自动生成连接  <http://www.github.com/>
    

`Control + Shift + L` 可插入Markdown语法。  
如果是 MWeb 的文档库中的文档，拖放或`CMD + Option + I` 导入非图片时，会生成连接。  
效果如下：

Email 连接： [example@example.com](mailto:example@example.com)  
[连接标题Github网站](http://github.com)  
自动生成连接像： [http://www.github.com/](http://www.github.com/) 这样

区块引用
----

Markdown 语法：

    某某说:
    > 第一行引用
    > 第二行费用文字
    

`CMD + Shift + B` 可插入Markdown语法。  
效果如下：

某某说:

> 第一行引用  
> 第二行费用文字

行内代码
----

Markdown 语法：

    像这样即可：`<addr>` `code`
    

`CMD + K` 可插入Markdown语法。  
效果如下：

像这样即可：`<addr>` `code`

多行或者一段代码
--------

Markdown 语法：

    ```js
    function fancyAlert(arg) {
      if(arg) {
        $.facebox({div:'#foo'})
      }
    }
    ```
    

`CMD + Shift + K` 可插入Markdown语法。  
效果如下：

    function fancyAlert(arg) {
        if(arg) {
            $.facebox({div:'#foo'})
        }
    }
    

顺序图或流程图
-------

Markdown 语法：

    ```sequence
    张三->李四: 嘿，小四儿, 写博客了没?
    Note right of 李四: 李四愣了一下，说：
    李四-->张三: 忙得吐血，哪有时间写。
    ```
    ```flow
    st=>start: 开始
    e=>end: 结束
    op=>operation: 我的操作
    cond=>condition: 确认？
    st->op->cond
    cond(yes)->e
    cond(no)->op
    ```
    

效果如下（ `Preferences` \- `Themes` \- `Enable sequence & flow chart` 才会看到效果 ）：

    张三->李四: 嘿，小四儿, 写博客了没?
    Note right of 李四: 李四愣了一下，说：
    李四-->张三: 忙得吐血，哪有时间写。
    

    st=>start: 开始
    e=>end: 结束
    op=>operation: 我的操作
    cond=>condition: 确认？
    st->op->cond
    cond(yes)->e
    cond(no)->op
    

更多请参考：[http://bramp.github.io/js-sequence-diagrams/](http://bramp.github.io/js-sequence-diagrams/), [http://adrai.github.io/flowchart.js/](http://adrai.github.io/flowchart.js/)

表格
--

Markdown 语法：

    第一格表头 | 第二格表头
    --------- | -------------
    内容单元格 第一列第一格 | 内容单元格第二列第一格
    内容单元格 第一列第二格 多加文字 | 内容单元格第二列第二格
    

效果如下：

第一格表头

第二格表头

内容单元格 第一列第一格

内容单元格第二列第一格

内容单元格 第一列第二格 多加文字

内容单元格第二列第二格

删除线
---

Markdown 语法：

    加删除线像这样用： ~~删除这些~~
    

效果如下：

加删除线像这样用： 删除这些

分隔线
---

以下三种方式都可以生成分隔线：

    ***
    *****
    - - -
    

效果如下：

* * *

* * *

* * *

MathJax
-------

Markdown 语法：

    块级公式：
    $$  x = \dfrac{-b \pm \sqrt{b^2 - 4ac}}{2a} $$
    \\[ \frac{1}{\Bigl(\sqrt{\phi \sqrt{5}}-\phi\Bigr) e^{\frac25 \pi}} =
    1+\frac{e^{-2\pi}} {1+\frac{e^{-4\pi}} {1+\frac{e^{-6\pi}}
    {1+\frac{e^{-8\pi}} {1+\ldots} } } } \\]
    行内公式： $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$
    

效果如下（`Preferences` \- `Themes` \- `Enable MathJax` 才会看到效果）：

块级公式：  
\\\[ x = \\dfrac{-b \\pm \\sqrt{b^2 - 4ac}}{2a} \\\]

\\\[ \\frac{1}{\\Bigl(\\sqrt{\\phi \\sqrt{5}}-\\phi\\Bigr) e^{\\frac25 \\pi}} =  
1+\\frac{e^{-2\\pi}} {1+\\frac{e^{-4\\pi}} {1+\\frac{e^{-6\\pi}}  
{1+\\frac{e^{-8\\pi}} {1+\\ldots} } } } \\\]

行内公式： \\(\\Gamma(n) = (n-1)!\\quad\\forall n\\in\\mathbb N\\)

脚注（Footnote）
------------

Markdown 语法：

    这是一个脚注：[^sample_footnote]
    

效果如下：

这是一个脚注：[1](#fn1)

注释和阅读更多
-------

Actions->Insert Read More Comment _或者_ `Command + .`  
**注** 阅读更多的功能只用在生成网站或博客时，插入时注意要后空一行。

TOC
---

Markdown 语法：

    [TOC]
    

效果如下：

*   [Markdown 的设计哲学](#toc_0)
*   [本文约定](#toc_1)
*   [标题](#toc_2)

*   [第一级标题 `<h1>`](#toc_3)
    
    *   [第二级标题 `<h2>`](#toc_4)
        *   *   *   *   [第六级标题 `<h6>`](#toc_5)
    *   [强调](#toc_6)
    *   [换行](#toc_7)
    *   [列表](#toc_8)
        *   [无序列表](#toc_9)
        *   [有序列表](#toc_10)
        *   [列表中嵌入代码块语法](#toc_11)
        *   [任务列表（Task lists）](#toc_12)
    *   [图片](#toc_13)
    *   [链接](#toc_14)
    *   [区块引用](#toc_15)
    *   [行内代码](#toc_16)
    *   [多行或者一段代码](#toc_17)
    *   [顺序图或流程图](#toc_18)
    *   [表格](#toc_19)
    *   [删除线](#toc_20)
    *   [分隔线](#toc_21)
    *   [MathJax](#toc_22)
    *   [脚注（Footnote）](#toc_23)
    *   [注释和阅读更多](#toc_24)
    *   [TOC](#toc_25)
    
    * * *
    
    1.  这里是脚注信息 [↩](#fnref1)