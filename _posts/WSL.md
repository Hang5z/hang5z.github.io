---
layout: default
title: Windows Subsystem for Linux
---

# Windows Subsystem for Linux

- [Windows Subsystem for Linux](#windows-subsystem-for-linux)
  - [一些基本概念](#一些基本概念)
    - [Windows NT](#windows-nt)
  - [OS/2](#os2)
  - [POSIX](#posix)
  - [Win32](#win32)
  - [WSL](#wsl)

## 一些基本概念

### Windows NT

**WinNT** 是一族由微软开发的操作系统，1993 年推出第一个版本。它是一种处理器独立、多处理器处理以及多用户的操作系统，采用**用户模式**和**核心模式**分层设计，并具备抢占式和可重入式的特点，利用 I/O 请求包和异步 I/O 来处理所有的 I/O 请求。

WinNT 是由 OS/2 衍生出来的，第一代 WinNT 为 Windows NT 3.1. 之后的 Windows 2000、Windows XP、Windows 7 到现在的 Windows 10 都是基于这一架构。

WinNT 最初是 32 位架构的，从 Windows XP 开始 64 位的 WinNT 开始出现。

## OS/2

OS/2 是由微软和 IBM 共同创造，后来由 IBM 单独开发的一套操作系统。2005 年， IBM 宣布不再销售和支持 OS/2 系统，也没有将源代码开源。

## POSIX

**POSIX**(Portable Operatng System Interface)，是一种 API 标准，X 表明其是对 Unix API 的传承。

Linux 基本上实现了 POSIX 兼容， WinNT 声称部分实现了 POSIX 标准。

## Win32

Win32 是 Windows API 的一个版本，为 Windows 95 之后的 Windows 操作系统提供 32 位的 API。这些 API 以系统 DLLs 的形式实现，核心 DLLs 包括 kernel32.dll、user32.dll以及 gdi32.dll。

Win32s 是 Win32 为 Windows 3.1x 操作系统族提供的一个 API 子集。

## WSL

WSL 是 Windows 10 提供的一种本地运行 unmodified Linux ELF64 binaries 的机制。

LX Session Manager Service 控制整个 Linux 实例的生命周期。

LXCore 和 lxss，即 lxcore.sys 和 lxss.sys，作为 WSL 的内核驱动，将 Linux 的系统调用翻译为对应的 NT API。当 Linux 系统调用在 NT API 中没有对应的映射时，它们将直接处理对应的请求。更详细内容可参见 [WSL System Calls](https://blogs.msdn.microsoft.com/wsl/2016/06/08/wsl-system-calls/)。

Pico 进程 是 Linux ELF 二进制文件的运行环境，它负责将可执行的 ELF 二进制文件加载到 Pico 进程的地址空间中，并在 Linux 兼容的系统调用层上执行它们。更详细内容可以参阅 [Pico Process Overview](https://blogs.msdn.microsoft.com/wsl/2016/05/23/pico-process-overview/)。

![wsl](pics/wsl/wsl.png)

------

WSL 不是虚拟机机制，因此也可以在 WSL 中运行 Windows 二进制。

```bash
netstat.exe -ano | grep 3306
netstat -nlt | FindStr.exe 3306
```

甚至可以启动 X 应用。

WSL 采用自己的 init system，因此无法调用 `systemctl` ，但可以通过 `service` 命令来进行服务管理。

WSL 和 Windows 环境共用端口(port)。在 WSL 下无法查看端口情况，也无法查看到 Windows 进程。 Windows 下可以看到 WSL 中运行的进程。

WSL 目前不支持 cron。

------

WSL 采用两种不同的文件系统来处理 Linux 下的文件(VolFs)和 Windows 下的文件(DriveFs)。

DriveFs 支持文件系统间的互操作，因此可以直接在 WSL 中访问 Windows 中的盘符，如 C 盘挂载在 `/mnt/c` 挂载点下。

VolFs不支持文件系统间的互操作，因此不要在 Windows 环境中访问 WSL 的文件，这可能造成错误。