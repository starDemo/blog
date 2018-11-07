title: Blog同步备份至Gitee
author: star liu
tags: []
categories: []
date: 2018-11-06 22:04:00
---
配置git repo，设置git同步数据至gitee，进行博客数据备份。
<!--more-->
- git repo拖动下载后执行npm i安装必备组件，可以方便的进行blog迁移。
- Hexo的方案讲真好评，这篇文章目的测试服务器自动更新。
- 新增一行进行测试。
- 脚本测试通过，更新时间戳commit msg+++bug fix
- 时间戳变量修复，添加单引号（osx Linux）（win下双引号）
- 调整时间格式兼容git commit message
- 脚本修改完成添加内容等待自动同步（目前每小时同步一次）