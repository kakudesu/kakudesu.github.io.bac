---
layout: post
title: 启用 Windows Sandbox
author: suki
date: 2018-12-26
tags:
- windows
- sandbox
english: false
---

# 启用Windows Sandbox

微软宣布将在明年的 Windows 10 19H1 更新中加入 Windows Sandbox 轻量化虚拟机功能，支持 Windows 10 专业版和企业版。Windows Sandbox 将为 Windows 管理员提供一个临时桌面环境，可以安全地测试不受信任的软件。

> Windows Sandbox 中安装的任何软件都只保留在沙箱中，不会影响您的主机。一旦 Windows Sandbox 关闭，所有具有文件和状态的软件将被永久删除。

### Windows Sandbox 特性

- 原生：作为 Windows 的一部分，此功能所需的一切资源，都随 Windows 10 专业版 / 企业版一起提供，无需专门去下载 VHD 虚拟机；
- 初始：每次运行 Windows Sandbox 时，运行环境都像全新安装的 Windows 系统一样干净；
- 安全：基于硬件虚拟化和内核隔离，后者依靠微软虚拟机管理程序运行单独的内核，将 Windows Sandbox 与主机隔离开来；
- 高效：使用集成的内核调度程序，智能内存管理，以及虚拟 GPU；
- 一次性：设备上不会遗留任何东西，关闭应用程序后，一切都将被丢弃。

### 先决条件

- Windows 10 专业版 或 企业版 (版本号 18305) 及更新版本
- 支持 AMD64 架构
- 已经在 BIOS 中启用 虚拟化
- 至少 4GB 内存 (推荐 8GB)
- 至少 1GB 可用磁盘空间 (推荐使用SSD)
- 至少 2 个 CPU 核心 (推荐使用4核超线程核心)

## 开启方法

- 安装 Windows 10 专业版 或 企业版 (版本号 18305) 或更新版本

- 启用虚拟化

* 如果您使用的是物理机，请确保在BIOS中启用了虚拟化功能。

* 如果您使用的是虚拟机，请使用此PowerShell cmdlet启用嵌套虚拟化：

```
Set-VMProcessor -VMName <VMName> -ExposeVirtualizationExtensions $ true*
```

- 打开Windows功能，然后选择Windows Sandbox。选择 “确定” 以安装Windows Sandbox。系统可能会要求您重新启动计算机。

- 打开开始菜单，找到 Windows Sandbox，运行即可