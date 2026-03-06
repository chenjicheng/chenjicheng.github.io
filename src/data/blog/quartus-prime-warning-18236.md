---
author: ch
pubDatetime: 2024-01-02T00:00:00Z
title: 'Quartus Prime 消除 "Warning (18236): Number of processors has not been specified..." 警告'
featured: false
draft: false
tags:
  - Quartus Prime
  - FPGA
description: "消除 Quartus Prime 中 Warning (18236) 处理器数量未指定的警告"
---

## Table of contents

## 解决办法
在 Quartus 安装目录下的 `assignment_defaults.qdf` 添加如下内容
```
set_global_assignment -name NUM_PARALLEL_PROCESSORS ALL
```
## 路径
星号为你安装时所选的路径，实在找不到请使用软件查询该文件的具体位置。
```
assignment_defaults.qdf
```
Windows
`*\quartus\bin64\assignment_defaults.qdf`
Linux
`*/quartus/linux64/assignment_defaults.qdf`

参考链接：[https://electronics.stackexchange.com/a/301251](https://electronics.stackexchange.com/a/301251)
