---
title: 开启Linux下Vmware Workstation的3D加速功能
url: 3495.html
id: 3495
categories:
  - 未分类
date: 2017-03-27 11:01:53
tags:
---

\[av\_textblock size='' font\_color='' color=''\] 最近兴起，主力机器主系统转移到了LinuxMint Mate17.3下，但是 Win环境又有部分需求，双系统重启比较麻烦 采用虚拟机方案， 然后 Win8的开始桌面 没有3D加速 哇 掉帧到爆炸，虽然虚拟机配置里打开了3D加速，但是开机报告"No 3D support is available from the host". 尴尬。

去http://askubuntu.com/查了相关帖子  得到了帮助
使用Vim编译`~/.vmware/preferences` 查找下 应该没有这项配置‘`mks.gl.allowBlacklistedDrivers`’
最后一行加上`mks.gl.allowBlacklistedDrivers = "TRUE"` 重启Vmware 就可以流畅的享受3D加速的虚拟机了

\[/av_textblock\]