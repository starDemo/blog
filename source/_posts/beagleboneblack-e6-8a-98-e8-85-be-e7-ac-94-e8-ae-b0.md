title: BeagleBoneBlack折腾笔记（一）
url: 3591.html
id: 3591
categories:
  - C
  - Linux
  - 未分类
  - 杂七杂八技术
date: 2018-04-08 14:18:02
tags:
---
能折腾就是RT 买买买 &&拆拆拆&&研究研究研究 新玩具
<!--more-->

## BeagleBoneBlack 
Ti 的AM335X处理器，性能不高，不比树莓派 全志系列的Pi，但是Ti爸爸的资料全以及后边貌似做了SIP方案基于这个片子，所以咸鱼入一个折腾折腾 研究下嵌入式Linux。 下边正题： 首页丢参数图 ![BBvsBBB.jpg](https://elinux.org/images/f/f8/BBvsBBB.jpg) 板子特性 ![Features.jpg](https://elinux.org/images/2/2b/Features.jpg) （拖自[Wiki](https://elinux.org/Beagleboard:BeagleBoneBlack) 无卵用，详情点[Wiki](https://elinux.org/Beagleboard:BeagleBoneBlack)飞过去） 研究内建驱动的模块 参考[帖子](https://blog.csdn.net/wyt2013/article/details/16874823)看到不少东西 基本就是几个路径的内建模块

*   /lib/modules/\*uname -a\*/kernel/drivers/
*   /sys/bus/i2c /sys/bus/spi

详细使用方法跳转看 空了整理 搬运这样这段话 ~~~~

另外关于/sys目录下的内容简单记录一下：

在 /sys 目录下有 block, bus, class, dev, devices, firmware, fs, kernel, module, power 这些子目录。 devices目录是这里面最重要的目录，这是内核对系统中所有设备的分层次表达模型。 block、bus、class、dev这4个目录分别是所有的块设备、按总线分类、按类别分类和按主次设备号分类的设备列表。实际上这4个目录的内容最后都是指向devices目录里相应设备的链接。相当于给我们提供了几种不同的方式找到想要找的设备。 其他目录的内容如下：

module目录里显示着几乎所有已经加载的驱动模块的信息，包括以内联方式编译到内核映像文件中的模块和外部模块(ko文件)。 kernel目录是内核所有可调整参数的位置，目前只有几项较新的设计在使用它，其它内核可调整参数仍然位于 sysctl (/proc/sys/kernel) 接口中。 fs目录按照设计是用于描述系统中所有文件系统，但目前只有少数文件系统支持 sysfs 接口，一些传统的虚拟文件系统(VFS)层次控制参数仍然在 sysctl (/proc/sys/fs) 接口中。 firmware目录是系统加载固件机制的对用户空间的接口。 power目录是系统中电源选项，这个目录下有几个属性文件可以用于控制整个机器的电源状态，如可以向其中写入控制命令让机器关机、重启等。

~~~~