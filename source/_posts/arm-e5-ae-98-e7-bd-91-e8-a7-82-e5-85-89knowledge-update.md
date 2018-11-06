---
title: ARM官网观光Knowledge Update
url: 3557.html
id: 3557
categories:
  - 未分类
date: 2017-12-09 21:31:25
tags:
---

RT 脆皮鸡配合cnblog舒服 [https://www.arm.com/products/processors/](https://www.arm.com/products/processors/) cortex-a/cortex-r/cortex-m [https://developer.arm.com/technologies/big-little](https://developer.arm.com/technologies/big-little) [https://developer.arm.com/technologies/neon](https://developer.arm.com/technologies/neon) 注意到了这句话

Tools
-----

[Arm DS-5 Development Studio](https://developer.arm.com/products/software-development-tools/ds-5-development-studio/editions) provides an end-to-end suite of tools for embedded C/C++ software development on a big.LITTLE SoC. 嗯 查工具！  

[ARM 开发工具 Keil和DS-5的比较。](http://www.cnblogs.com/njseu/p/6025409.html)
---------------------------------------------------------------------

2016-11-03 09:51 by NJSEU, 5234 阅读, 0 评论, [收藏](https://www.cnblogs.com/njseu/p/6025409.html#), [编辑](https://i.cnblogs.com/EditPosts.aspx?postid=6025409)

http://www.eeboard.com/bbs/thread-25219-1-1.html

如今ARM体系架构的处理器在嵌入式市场上呼风唤雨，从低端的MCU应用到高端的多媒体消费电子，移动设备领域，工业控制，医疗设备，汽车电子等，到处是ARM架构处理器大军的身影。

ARM开发工具就是ARM公司为庞大的各领域工程师和开发人员装备的完整的开发工具链，帮助迅速搭建开发平台，降低开发的成本和难度，缩短开发周期，让工程师们尽情享用ARM架构处理器这道‘饕餮大餐’。

这里我将针对ARM开发工具的各种产品分类及功能特性做一个详细的讲解和对比，希望能帮大家区分理解各个开发工具的优点特长，为自己的开发平台选择合适的开发工具。

 

ARM开发工具之DS-5篇

2011年随着ARM新发布几款新的Cortex-A系列内核和Big.LITTLE技术，以及即将推出的下一代ARMv8架构体系，RVDS已经渐渐无法满足新设备支持和日益复杂的Soc系统开发的需求，应机而推出的的DS-5在以下几个功能有特别的亮点.

1.      ARM原厂提供的ARM CC编译器，有效

2.      支持最所有的ARM内核，包括Cortex-A15/A7/A12，Cortex-M/R系列以及新增的对A50即ARMv8架构的支持。

3.      支持Big.LITTLE多核调试技术。

4.      更为灵活，强大和使用简单的调试器，支持Linux/Andriod系统内核调试(驱动开发由此变得简单了)。

5.      应用程序性能分析器Streamline,有效分析应用代码执行效率，简单易懂的图形化表示，帮助改善代码性能和瓶颈。

6.      支持新一代的高速调试仿真器DSTREAM,支持4GB的Trace 空间。

<ignore\_js\_op>![DS-5功能框图](http://www.eeboard.com/bbs/data/attachment/forum/201401/15/1141296e4y93l1llhhlso5.jpg "DS-5功能框图") 图1  DS-5功能框架

        ARM看上了移动消费市场的巨大蛋糕，日渐复杂的Soc系统开发和多核处理器更新，产品开发的成本更多是发生在软件开发阶段，在DS-5在增加了Linunx/Andriod系统内核调试满足市场对复杂Soc系统开发的要求，图形化代码性能分析器streamline等重要的功能更是让工程师节省了大量的开发的时间，有效的突破了软件开发的瓶颈，帮助产品更快的推向市场，大大的降低了软件开发环节的成本。对于快速更新换代的移动消费设备而言，最快的推出新产品才能在这个市场保持做大称霸。根据我们日常的销售数据统计，几个主要的半导体原厂高通，华为，三星，全志，炬力等都在全面更新DS-5开发工具了，所以下游的ODM/OEM,手机平板及移动设备开发商很快就将随着半导体原厂工具平台更新使用DS-5开发工具。

我们的FAE支持人员每天忙着跑华为高通的实验室支持他们使用DS-5和DSTREAM，并为这些原厂建立了支持热线，协助他们更新DS-5开发工具平台，支持调试他们最新发布的CPU，DS-5通过众多半导体巨头用户证明了自己的强大和有效，DS-5将被环绕在这些巨头中间大放异彩，成为行业开发工具的顶级明星！

如果各位ARM嵌入式工程师想进入华为，高通等高薪大企业，提前学好DS-5吧，这绝对是你打动面试官最有说服力的理由！

                                                <ignore\_js\_op>![Streamline](http://www.eeboard.com/bbs/data/attachment/forum/201401/15/111602fgb6vqgjffajvbcq.jpg "Streamline")

图2 Streamline功能截图

由于篇幅所限我将另行起文，图文并茂的解析DS-5，记得来关注哦！

 

ARM开发工具之KEIL篇

     谈起KEIL，相信上点年纪的单片机工程师都会有种初恋的感觉湿润了眼眶，多少个日夜，这个界面uVision IDE陪伴着年轻的你熬夜奋斗到天明，多少个分分秒秒，她陪伴你从一个初级的单片机菜鸟努力慢慢成长为一个经验丰富的嵌入式工程师…..来个玉照回味一下。

![KEIL2](http://www.eeboard.com/bbs/data/attachment/forum/201401/15/114146g1rwav08ftrt36n8.jpg "KEIL2") 图3 KEIL uVision2  启动界面  ![KEIL3](http://www.eeboard.com/bbs/data/attachment/forum/201401/15/111931rztss2j2ictdztgd.jpg "KEIL3") 图4 KEIL MDK 启动界面

         KEIL 是得到了超过10万名资深工程师认可的世界领先的开发工具，它具有强大的功能和易用的开发环境，通过多年的积累，KEIL在广大的MCU应用领域拥有庞大的忠实用户群体，对于喜欢死磕的工程师而言，KEIL uVision就是初恋，那就是真爱…………

      正准备大举进军MCU市场的ARM和KEIL公司两眼一相望，基情迸发，那是如胶似漆,恩爱到白头…..。

2005年10月，ARM正式全资收购KEIL，把KEIL工具纳入自己的工具链体系，帮助现有的8/16位工程师群体顺利转移到ARM 32位Cortex-M平台上，这也是后来KEIL MDK 后来为什么会有Realview标识的原因，请看上图对比。

           当时ARM的收购声明这样说到:”ARM确认MCU市场将会是极为重要的业务增长方向，通过这次的收购建立一个完善和更具说服力的解决方案帮助ARM加速在这个MCU市场的进军；伴随MCU应用正从8/16位的解决方案向32位的发展，我们专门为微处理应用器定制的Cortex-M 系列处理器加上高性能的Realview 编译器和KEIL的MUC工具链补充，将开辟新一代的ARM MCU 解决方案”

MDK(Microcontroller Development Kit)就是作为KEIL支持ARM设备的版本名称。

与MDK对应的原来的KEIL系列分别是

C51 开发工具系列(C51Development Tools) 是支持8051系列微处理器的编译开发工具，支持所有主要半导体厂商

的8051系列新品。C51开发工具系列包含A51汇编器，CA51编译器(含汇编器A51)和PK51。

  PK51的全称是PK51 Professional   Developer’s Kit,包含CA51编译器，调试器，Hex转换器等等。所以从这描述可以看出，

  PK51 是最全的C51开发工具，A51和CA51都是其中的一个部分而已，各位没用过KEIL uVision的采购人员应该明白这几个

  产品之间的关系了吧。

C166 开发工具系列(C166 Development Tools) 是支持XC16x,C16x, 和 ST10 系列微处理器的开发工具。 C166系列

   又包含 A166，CA166和PK166，它的区分原理和C51系列一样，这里就不再赘述。

DK251开发工具系列(C251Development Tools) 是支持 251微处理器架构系列的开发工具。

 

MDK作为KEIL工具里的主角，这里我们重点描述一下它的功能优点。

1\. 支持超过900多种设备，包含ARM7/ARM9，ARM Cortex-M系列等体系架构的CPU,几乎市场上半导体原厂出这几种架构

   CPU 都支持。自从有了她，你也再不用因为换芯片平台而找新的开发工具而苦恼了。

2\. 早已被我们熟知的uVision IDE环境友好易用，功能强大，目前升级到uVision5更是焕然一新。

3\. 全功能的RTOS实时操作系统RTX,提供源码。

4\. 广泛的中间库支持，帮助用户很容易的搭建起辅助的网络连接和通信系统。

5\. 支持广泛的硬件调试器和第三方开发工具，什么Ulink2/ulinkpro/ST-link/Jlink等。其中通过UlinkPro完成对硬件实        时跟踪和代码分析功能。

6.完整的代码覆盖识别每一条指令，保证你的程序代码稳健。

7.包含丰富的例程代码还有最重要的启动文件。MCU的启动文件极为复杂，需要汇编配置好内核，时钟和初始化，

  如果没有KEIL MDK的启动代码，估计我等小菜鸟估计在写启动文件阶段就挂机了，更别谈写个hello word出来了。

8. 极为出色的代码性能分析器帮助工程师找到程序应用的瓶颈，提高改善软件的性能。这一条文字看来很苍白， 我们来看看几个美图带来的视觉冲击吧 <ignore\_js\_op>![KEIL性能分析](http://www.eeboard.com/bbs/data/attachment/forum/201401/15/132434jt289puotgvu23t3.jpg "KEIL性能分析") <ignore\_js\_op>![KEIL 事件触发记录](http://www.eeboard.com/bbs/data/attachment/forum/201401/15/1332220qtgas304rz4vvbj.jpg "KEIL 事件触发记录") 图5 MDK代码性能分析                                       图6 MDK 事件触发执行记录

   <ignore\_js\_op>![MDK Trace](http://www.eeboard.com/bbs/data/attachment/forum/201401/15/133543xe8xnr8zsnjbe8ex.jpg "MDK Trace")

图7 MDK Trace 跟踪数据 看到这些图你有没顿时觉得一切bug都是浮云了，代码执行和事件触发都trace保存下来了，所有的寄存器和可访问地址内存都可以查看，可以查询函数引用堆栈，可以精确的定位到程序跑飞的那一行代码，可以通过函数调用次数频率有效的优化代码，顿时觉得这个世界都很美好了，有没有这种感觉？

由于篇幅原因，我将另行起文详细介绍MDK的功能，图文并茂的那种，欢迎大家关注。

 

ARM开发工具之DS-5 vs MDK对比篇

如果说DS-5是高大全，那MDK就是高富帅，两者都是ARM开发工具里重要的角色，但是似乎两者都用共同的功能和优点，让大家一时难以理解为什么ARM要同时保持这两种大杀器的存在，是故意让大家多纠结取舍吗? 这里请大家看一下两者的主要功能差异对比

<ignore\_js\_op>![MDK-VS-DS5](http://www.eeboard.com/bbs/data/attachment/forum/201401/15/152932tyvkt47wck7ykfy8.jpg "MDK-VS-DS5")

图8   MDK 和 DS-5 功能对比

     通过上图对比我们很容易就能理解，KEIL MDK-ARM是用于满足开发者基于ARM7/9,ARM Cortex-M 处理器的开发需求，包括它自带的RTX实时操作系统和中间库，都是属于MCU应用领域的。

         DS-5是用于创建Linux/Andriod的复杂嵌入式系统应用和系统平台驱动接口，DS-5支持设备添加，包括多核调试和支持，主要针对复杂的多核调试，片上系统开发而推出的。

     换而言之，没有最好的工具，只有最合适的工具，如果你要做MCU应用，我推荐你用KEILMDK,如果你要做片上系统，Linux/Andriod驱动和应用开发，那我推荐你使用DS-5+DSTREAM。