# 🖱️ DoubleClickFix - 中文版 (双击修正)

[![.NET](https://github.com/nenning/DoubleClickFix/actions/workflows/dotnet.yml/badge.svg)](https://github.com/nenning/DoubleClickFix/actions/workflows/dotnet.yml) &nbsp; [![GitHub release (latest by date)](https://img.shields.io/github/v/release/nenning/DoubleClickFix)](https://github.com/nenning/DoubleClickFix/releases/latest) &nbsp; [![License](https://img.shields.io/github/license/nenning/DoubleClickFix)](LICENSE.txt)

这是一个轻量级的 Windows 工具，专门用于修复由于鼠标开关硬件故障（如：微动开关连击、抖动）引起的意外双击问题。

**版本 1.5 更新：** 🖱️ **鼠标滚轮修复** – 如果您的鼠标滚轮存在“回滚”或“抖动”问题，现在可以在 UI 中开启修复功能！

**版本 1.4 更新：** 🎉 **实验性拖拽支持** – 修复拖拽过程中由于硬件连击导致的意外松开。开启后，工具会保持稳定的拖拽状态，直到您明确释放。

此工具通过拦截并过滤非正常的双击事件来确保操作流畅，并支持可靠的拖拽手势。您可以在直观的用户界面中直接定义有效点击之间的最小延迟。

## ✨ 主要功能

- **拖拽锁定修复 (新!)**：在 UI 中开启。即使您的鼠标开关在拖动过程中发生抖动，也能保持稳定的拖动状态。拖动时的短暂停顿将被视为真正的释放，防止意外掉落。
- **鼠标滚轮修复**：过滤掉异常的滚轮事件，防止意外反向滚动。
- **自定义延迟**：通过用户友好的界面调整两次点击之间的最小毫秒数。默认值为 50ms。
- **针对特定按键进行设置**：支持左键、右键、中键、X1 和 X2 键。默认仅对左键生效。
- **Windows 系统托盘集成**：双击托盘图标即可打开设置界面，平时静默运行。
- **开机启动选项**：支持注册为 Windows 开机启动项。

---

## 🔍 工作原理：过滤鼠标点击

本项目通过底层钩子拦截鼠标事件，以区分正常的点击和由于鼠标硬件老化产生的“震荡（Chattering）”：

1.  **底层鼠标钩子 (Low-Level Mouse Hook)**：注册 `WH_MOUSE_LL` 钩子，在鼠标点击信号传递给系统和其他应用之前进行拦截。
2.  **点击过滤**：当检测到按下事件时，计算与上一次弹起事件的时间差。如果小于用户设置的阈值（如 50ms），则视为无效点击并“吞掉（Swallow）”该信号。
3.  **拖拽矫正**：针对拖拽过程中的抖动，进入“拖拽锁定”模式，过滤掉中间产生的意外弹起信号，直到由于静止而判定为真实释放。
4.  **滚轮防抖**：通过方向感知技术过滤掉短时间内产生的反向滚动跳变。

---

## 🖥️ 系统要求

- **操作系统**：Windows 10 或更高版本。
- **运行环境**：[.NET 8.0 Desktop Runtime](https://dotnet.microsoft.com/zh-cn/download/dotnet) 或更高版本（通常会自动安装）。

---

## 🚀 安装与使用

1.  **下载**：从 [Releases 页面](https://github.com/nenning/DoubleClickFix/releases) 下载最新发布的压缩包。
2.  **运行**：解压并执行 `.exe` 文件即可。
    - 注意：设置存储在注册表：`HKEY_CURRENT_USER\Software\DoubleClickFix`。

---

## 🎮 反作弊软件兼容性 (VAC, EAC, BattlEye 等)

本项目使用 Windows 标准的物理输入层钩子 (`WH_MOUSE_LL`)。它**不**：
- 注入任何代码到其他进程。
- 读取或修改任何游戏的内存。
- 自动化游戏操作（宏或连点器）。

虽然风险极低，但没有任何第三方工具能保证 100% 不受反作弊系统影响。本工具属于无害的辅助工具，用于解决硬件寿命带来的困扰。

---

## 🛠️ 技术说明

本项目是基于作者 [nenning/DoubleClickFix](https://github.com/nenning/DoubleClickFix) 的汉化修改版。

### 🌍 语言强制切换
如果您想切换回英文或其他语言，可以在 `app.config` 文件中通过修改 **`languageOverride`** 键值来实现。

---

## 📜 开源协议
本项目根据 [MIT 协议](LICENSE.txt) 分发。
