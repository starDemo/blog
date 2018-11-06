---
title: ESP8266 NodeMCU刷固件时的反思
url: 3597.html
id: 3597
categories:
  - 未分类
  - 杂七杂八技术
  - 杂侃
date: 2018-05-09 00:33:25
tags:
---

淘宝买的某山寨WEMOS NodeMCU评估板子，烧录固件总是报错，

Timed out waiting for packet header

或者各种其他错误，总是刷不进去。尝试过更换固件，更换下载软件 NodeMCUDownload，esptool,FlashDownload都错误，后爬问发现这样一句。 ······

Most likely the ESP is not being put into flash mode. If it has buttons, hold flash, and then press reset, then run the tool. But you haven't mentioned which ESP module you use, and in what setup. ········ 嗯，长按Flash按键，烧录成功。