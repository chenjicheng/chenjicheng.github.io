---
author: ch
pubDatetime: 2024-01-03T00:00:00Z
title: "Quartus Prime Simulation waveform 修复 Error (suppressible): (vsim-12110) 及 signal not found in VCD"
featured: false
draft: false
tags:
  - Quartus Prime
  - FPGA
  - 仿真
description: "修复 Quartus Prime 仿真中 vsim-12110 错误和 signal not found in VCD 警告的方法"
---

# Fuck Intel
**注意！本篇文章已经违反 EULA（最终用户许可协议），本篇内容所涉及的操作将会违反该软件 EULA 中的以下内容以及可能违反其他我没仔细看的内容！**
```
2.2.3. You may not use, copy, modify, distribute, or otherwise
transfer the Licensed Software or any portions thereof, or permit any
remote access thereof by any person or entity.

2.2.7. You may not decompile, disassemble, reverse engineer, or
otherwise attempt to access the source code of the Licensed Software
or reduce it to a human readable form except as otherwise permitted by
applicable law.

7.3. Notwithstanding anything else in the Agreement, Intel will not
indemnify or defend You for claims asserted, in whole or part,
against: (i) technology or designs that You gave to Intel; (ii)
failure to use compatible devices (including Intel Devices) as set
forth in the Specifications;  (iii) modifications or programming to
the Licensed Software that were made by anyone other than Intel; or,
(iv) the Licensed Software's alleged implementation of some or all of
a Standard.
```
此问题会导致仿真无法正常使用！

不删除脚本中的 `-novopt` 会导致出现如下报错
```log
# ** Error (suppressible): (vsim-12110) All optimizations are disabled because the -novopt option is in effect. This will cause your simulation to run very slowly. If you are using this switch to preserve visibility for Debug or PLI features, please see the User's Manual section on Preserving Object Visibility with vopt. -novopt option is now deprecated and will be removed in future releases.
# Error loading design
```
不给 `vsim` 命令添加 `-voptargs=+acc` 会导致出现如下警告，且无法输出仿真波形，实际情况中星号处会显示你要仿真的信号名。
```
Warning: * - signal not found in VCD.
```

Altera 作为 FPGA 界的老二对自家的软件实在是不够上心，虽然是免费版，但出现这种严重影响体验的 bug 实在是不应该，而且好几年都没有修复，不知道这个问题折磨了多少人。

至于怎么找到要修改这个位置的，因为在反编译这块我也只是懂一点三脚猫功夫，就不露怯了。
# 开始修复
## 省流
使用 Windows 版本 `Quartus Prime Version 23.1std.0 Build 991 11/28/2023 SC Lite Edition` 的小伙伴可以直接下载我修改过的 DLL，其他版本不保证能用。此 DLL 只是将脚本中的字符串 `-novopt` 替换为 `-voptargs=+acc`，如不放心请参考下面的教程进行修复。

直接拖进 Quartus 可执行文件根目录中替换原文件即可。默认安装目录如下

```path
C:\intelFPGA_lite\23.1std\quartus\bin64
```

下载链接：[https://www.123pan.com/s/ZDUWjv-WQsod.html](https://www.123pan.com/s/ZDUWjv-WQsod.html)

## 下载 DIE-engine
首先下载 `DIE-engine`

[https://github.com/horsicq/DIE-engine/releases](https://github.com/horsicq/DIE-engine/releases)

下载后将文件解压到一个文件夹中，解压完成后打开 `die.exe`，打开后勾选 `高级选项`。

![DIE 高级选项](/images/vsim-fix/die-advanced-options.gif)
## 复制 DLL 文件
由于默认安装在系统目录中，需要以管理员权限启动 DIE（DIE-engine）才能直接修改。另外，为了安全起见也建议先复制两份，一份作为备份，另一份用于修改。

Windows 下的默认路径为

```
C:\intelFPGA_lite\23.1std\quartus\bin64
```

在此路径下搜索即可，文件名为

```
edt_wedtq.dll
```

## 修改 DLL 文件
直接将 DLL 文件拖入刚才打开的 DIE 窗口，或点击 DIE 右上角的三个小点![手动选择](./images/vsim-fix/die-select-file.png)手动选择路径。

扫描完成后点击 `字符串` 按钮即可搜索字符串并替换，替换前需取消勾选 `只读`，具体操作如下图所示。将 `-novopt` 替换为

```text
-voptargs=+acc
```

![修改 DLL](/images/vsim-fix/die-replace-string.gif)
最后把修改完成的 DLL 文件拖到上述目录覆盖原文件即可。

参考链接

[https://1ri.dev/2022/2GP5YGS/](https://1ri.dev/2022/2GP5YGS/)

[https://verimake.com/d/247-quartusquesta](https://verimake.com/d/247-quartusquesta)

[https://users.ece.cmu.edu/~jhoe/doku/doku.php?id=a_short_intro_to_modelsim_verilog_simulator](https://users.ece.cmu.edu/~jhoe/doku/doku.php?id=a_short_intro_to_modelsim_verilog_simulator)

[https://community.intel.com/t5/Intel-Quartus-Prime-Software/Questa-option-voptargs-quot-acc-quot/td-p/1362284](https://community.intel.com/t5/Intel-Quartus-Prime-Software/Questa-option-voptargs-quot-acc-quot/td-p/1362284)
