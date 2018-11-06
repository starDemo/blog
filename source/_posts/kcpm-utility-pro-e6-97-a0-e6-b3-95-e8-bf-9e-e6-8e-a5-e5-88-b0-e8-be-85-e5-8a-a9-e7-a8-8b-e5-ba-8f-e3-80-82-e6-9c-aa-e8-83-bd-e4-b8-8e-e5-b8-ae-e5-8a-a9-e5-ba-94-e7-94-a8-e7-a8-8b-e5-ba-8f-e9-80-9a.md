---
title: KCPM Utility Pro 无法连接到辅助程序。 未能与帮助应用程序通信。
url: 3540.html
id: 3540
categories:
  - 杂七杂八技术
date: 2017-09-20 00:51:07
tags:
---

RT蛇皮Bug. 我还以为系统不兼容md 已经解决了，又看了官网的说明： Q4. An alert saying that KCPM Utility Pro cannot connect to the helper after I typed my password. A4. Quit the KCPM Utility Pro, run the KCPMP5Remover and restart the KCPM Utility Pro. Instead of running the KCPMP5Remover, you can try to manually do the following things and restart the KCPM Utility Pro. **Delete /Library/PrivilegedHelperTools/science.firewolf.kcpmp.helper** **Delete /Library/LaunchDaemons/science.firewolf.kcpmp.helper.plist** Kill the process named science.firewolf.kcpmp.helper via terminal or Activity Monitor.app.