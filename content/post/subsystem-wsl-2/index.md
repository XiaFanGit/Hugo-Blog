---
title: 关于 WSL 2 使用的经验谈
description: 与 WSL 1 相比，WSL 2 做了架构的巨大变更，使用了虚拟化技术。从现在开始，WSL 2 拥有了完整的 Linux 内核，支持 Docker，GPU 等 WSL 1 时代无法实现的功能，并能够与 Visual Studio Code 集成。
date: 2020-09-09
slug: subsystem-wsl-2
image: wsl2.png
categories:
    - WSL
    - Windows
    - Linux
---

### 先决条件

* Version 1903 or higher, with Build 18362 or higher.

### 开启虚拟化支持

在 Windows Features 中勾选 Virtual Machine Platform (虚拟机平台) 和 Windows Subsystem for Linux (适用于 Linux 的 Windows 子系统)

或者以管理员身份打开 PowerShell 并运行

```text
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

### 安装 Linux 内核更新程序

[WSL2 Linux kernel update package for x64 machines](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

### 设置 WSL 2 作为默认的版本

以管理员身份打开 PowerShell 并运行

```powershell
wsl --set-default-version 2
```

### 安装 CentWSL

Windows Store 中并没有提供 CentOS 7/8 的发行版，如果你想使用 CentOS 可以使用开源的 CentWSL。

https://github.com/wsldl-pg/CentWSL

1. 下载 Release 中的 Zip 安装包
2. 解压 Zip 包到你想安装 WSL 的位置
3. 运行 CentOS.exe 以初始化 rootfs 并且注册到 WSL
4. 你可以为 CentOS.exe 创建一个快捷方式

### 查看 WSL 的运行状态

```powershell
wsl -l -v
```

### 从 CMD/Powershell 进入到 WSL

```powershell
bash
```

### 为 VSCode 启用 WSL

在 VSCode 中安装 [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)

你可以从屏幕左下角的 >< 切换到 WSL:CentOS7

### 替换 Systemctl 

默认的 Systemctl 有一些问题，在 WSL 中执行以下命令进行替换

```bash
mv /usr/bin/systemctl /usr/bin/systemctl.old
curl https://raw.githubusercontent.com/gdraheim/docker-systemctl-replacement/master/files/docker/systemctl.py > /usr/bin/systemctl
chmod +x /usr/bin/systemctl
```
