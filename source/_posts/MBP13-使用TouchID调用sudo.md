title: MBP13 使用TouchID调用sudo
author: star liu
tags:
  - macos
categories:
  - 杂侃
date: 2019-09-19 11:28:00
---
macos 使用touchid代替sudo
<!--more-->
## 操作方法
打开“终端”，执行以下命令：
```
sudo sed -i ".bak" '2s/^/auth       sufficient     pam_tid.so\'$'\n/g' /etc/pam.d/sudo
sudo sed -i ".bak" '2s/^/auth       sufficient     pam_tid.so\'$'\n/g' /etc/pam.d/sudo
```
然后输入您的管理员密码，回车，大功告成了！不用重启哦～
## 命令说明
该命令的作用是把 `/etc/pam.d/sudo `备份为 `/etc/pam.d/sudo.bak`，然后在 `/etc/pam.d/sudo` 的第二行前面加入 `uth sufficient pam_tid.so `这个字符串。
修改该文件的目的是在 sudo 程序的认证过程前面插入 Touch ID 验证的模块。感兴趣的小伙伴可以去了解一下 PAM 架构。
如果需要恢复原文件，请执行：`sudo mv /etc/pam.d/sudo.bak /etc/pam.d/sudo`。

```
作者：hptuchief
链接：https://hacpai.com/article/1512017120360
来源：黑客派
协议：CC BY-SA 4.0 https://creativecommons.org/licenses/by-sa/4.0/
```