---
author: ch
pubDatetime: 2023-12-30T00:00:00Z
title: "沁恒 WCH CH341A 编程器在编程模式（并口）下显示未知设备解决办法"
featured: false
draft: false
tags:
  - 硬件
  - 驱动
description: "CH341A 编程器在编程模式下显示未知设备时，需安装正确的并口驱动"
---

沁恒官网下载的 [CH341SER.EXE][1] 是 USB 转串口驱动，需要下载 [CH341PAR.EXE][2] 才能使用 USB 转 JTAG/SPI/I2C/并口（编程）/GPIO 等模式。

[1]: https://www.wch.cn/downloads/CH341SER_EXE.html
[2]: https://www.wch.cn/downloads/CH341PAR_EXE.html
