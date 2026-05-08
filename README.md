<div align="center">

# CrabCode · 蟹码

**终端原生的 AI 编程助手**<br>
**A terminal-native AI coding assistant**

[![Latest Release](https://img.shields.io/github/v/release/acosmi/crabcode?display_name=tag&label=latest)](https://github.com/acosmi/crabcode/releases/latest)
[![Platform](https://img.shields.io/badge/platform-macOS%20%7C%20Linux%20%7C%20Windows-blue)](#当前发布)
[![Status](https://img.shields.io/badge/status-active-success)](https://github.com/acosmi/crabcode/releases)
[![Issues](https://img.shields.io/github/issues/acosmi/crabcode?label=issues)](https://github.com/acosmi/crabcode/issues)

[简体中文](#简体中文) · [English](#english)

</div>

---

## 限时福利 · Free Tokens Until May 31

> 新用户注册即送词元（tk）福利包，覆盖日常编码、对话、工具调用和思考模式。
>
> New users receive a free token (tk) bundle for coding, chat, tool calls, and thinking modes.

- 注册 / Sign up: <https://acosmi.com/zh>
- 截止 / Ends: **2026-05-31**
- 注册后在 CrabCode 中执行 `/login` 即可激活额度。
- After signup, run `/login` inside CrabCode to activate your quota.
- 老用户邀请新用户可额外获得词元奖励。
- Existing users can earn bonus tokens by referring new users.

---

## 简体中文

### 简介

**CrabCode（蟹码）** 是一款运行在终端里的 AI 编程助手。它把模型调用、代码检索、文件编辑、Shell 执行、MCP 工具和 GitHub 工作流放进一个统一的命令行体验里，让开发者可以在当前工程目录内完成代码理解、排错、重构、测试生成和自动化任务。

这个公开仓库用于发布稳定安装包和面向用户的说明。最新版本为 **v1.3.33**，发布时间为 **2026-05-08 UTC**。

### 当前发布

| 平台 | 架构 | 发布包 |
|---|---:|---|
| macOS Apple Silicon | arm64 | `crabcode-1.3.33-darwin-arm64.tar.gz` |
| macOS Intel | x64 | `crabcode-1.3.33-darwin-x64.tar.gz` |
| Linux | arm64 | `crabcode-1.3.33-linux-arm64.tar.gz` |
| Linux | x64 | `crabcode-1.3.33-linux-x64.tar.gz` |
| Windows | x64 | `crabcode-1.3.33-win-x64.zip` |

所有发布包都在 [Releases](https://github.com/acosmi/crabcode/releases/latest) 页面提供，并附带 SHA-256 校验文件。

### 核心能力

- **终端原生 TUI**：全屏交互、流式输出、历史会话、主题和中英文界面。
- **模型动态路由**：可用模型、能力和价格由 Acosmi SDK 动态下发；在客户端内使用 `/model` 查看和切换。
- **工程级工具**：读取、写入、精确编辑、全文检索、Glob 匹配、沙盒化 Shell、Notebook 读写。
- **MCP 生态**：支持配置和调用 Model Context Protocol 服务器，包括浏览器自动化、外部数据源和自定义工具。
- **工作流增强**：Hooks、Skills、Plan Mode、权限管理、Git / GitHub 协作、跨会话记忆。
- **定时任务**：`crabcode cron` 使用独立 Rust 调度守护进程，支持 macOS、Linux 和 Windows。

### 架构状态

CrabCode 当前是 **Rust 底层核心 + TypeScript 业务层**：

| 组件 | 作用 |
|---|---|
| `crabcode` | Rust CLI 启动器，解析高频参数并启动交互式客户端 |
| `dist/index.js` | TypeScript 业务层，负责 TUI、agent loop、模型 SDK、MCP 和工具编排 |
| `crabcode-cron` | Rust 定时任务守护进程；Unix 使用 socket，Windows 使用 Named Pipe |
| `dist/vendor/ripgrep` | 随包内置的 ripgrep，用于高速代码搜索 |

历史 Hub / Go 守护进程路径已下线；模型与账户能力通过 `@acosmi/sdk-ts` 直连 Acosmi 网关。

### 安装

#### macOS

选择你的平台：

- Apple Silicon: `darwin-arm64`
- Intel: `darwin-x64`

```bash
VERSION=1.3.33
PLATFORM=darwin-arm64

curl -fsSL -o /tmp/crabcode.tar.gz \
  "https://github.com/acosmi/crabcode/releases/download/v${VERSION}/crabcode-${VERSION}-${PLATFORM}.tar.gz"

rm -rf "$HOME/.local/share/crabcode"
mkdir -p "$HOME/.local/share/crabcode" "$HOME/.local/bin"
tar -xzf /tmp/crabcode.tar.gz --strip-components=1 -C "$HOME/.local/share/crabcode"

xattr -dr com.apple.quarantine "$HOME/.local/share/crabcode" 2>/dev/null || true
ln -sf "$HOME/.local/share/crabcode/crabcode" "$HOME/.local/bin/crabcode"

crabcode --version
crabcode
```

如果 `~/.local/bin` 不在 `PATH` 中，把下面一行加入 `~/.zshrc` 或 `~/.bashrc` 后重新打开终端：

```bash
export PATH="$HOME/.local/bin:$PATH"
```

#### Linux

选择你的平台：

- x64: `linux-x64`
- arm64: `linux-arm64`

```bash
VERSION=1.3.33
PLATFORM=linux-x64

curl -fsSL -o /tmp/crabcode.tar.gz \
  "https://github.com/acosmi/crabcode/releases/download/v${VERSION}/crabcode-${VERSION}-${PLATFORM}.tar.gz"

rm -rf "$HOME/.local/share/crabcode"
mkdir -p "$HOME/.local/share/crabcode" "$HOME/.local/bin"
tar -xzf /tmp/crabcode.tar.gz --strip-components=1 -C "$HOME/.local/share/crabcode"
if command -v xattr >/dev/null 2>&1; then
  xattr -dr com.apple.quarantine "$HOME/.local/share/crabcode" 2>/dev/null || true
fi
ln -sf "$HOME/.local/share/crabcode/crabcode" "$HOME/.local/bin/crabcode"

crabcode --version
crabcode
```

#### Windows

在 PowerShell 中执行：

```powershell
$Version = "1.3.33"
$Package = "crabcode-$Version-win-x64"
$Zip = "$env:TEMP\$Package.zip"
$InstallRoot = "$env:LOCALAPPDATA\crabcode"
$Bin = Join-Path $InstallRoot $Package

Invoke-WebRequest `
  -Uri "https://github.com/acosmi/crabcode/releases/download/v$Version/$Package.zip" `
  -OutFile $Zip

Remove-Item $Bin -Recurse -Force -ErrorAction SilentlyContinue
New-Item -ItemType Directory -Force -Path $InstallRoot | Out-Null
Expand-Archive -Path $Zip -DestinationPath $InstallRoot -Force

$CurrentPath = [Environment]::GetEnvironmentVariable("Path", "User")
if (($CurrentPath -split ";") -notcontains $Bin) {
  [Environment]::SetEnvironmentVariable("Path", "$CurrentPath;$Bin", "User")
}

& "$Bin\crabcode.exe" --version
```

重新打开 PowerShell 后运行：

```powershell
crabcode
```

#### 覆盖更新已安装版本

覆盖更新只替换程序文件，不会删除本地配置、登录状态、历史记录和记忆数据。不要删除 `~/.crabcode/`。

macOS / Linux 使用同一安装目录，重新执行对应平台的安装命令即可覆盖旧版本：

```bash
VERSION=1.3.33
PLATFORM=darwin-arm64  # macOS Intel 改为 darwin-x64；Linux 改为 linux-x64 或 linux-arm64

curl -fsSL -o /tmp/crabcode.tar.gz \
  "https://github.com/acosmi/crabcode/releases/download/v${VERSION}/crabcode-${VERSION}-${PLATFORM}.tar.gz"

rm -rf "$HOME/.local/share/crabcode"
mkdir -p "$HOME/.local/share/crabcode" "$HOME/.local/bin"
tar -xzf /tmp/crabcode.tar.gz --strip-components=1 -C "$HOME/.local/share/crabcode"
if command -v xattr >/dev/null 2>&1; then
  xattr -dr com.apple.quarantine "$HOME/.local/share/crabcode" 2>/dev/null || true
fi
ln -sf "$HOME/.local/share/crabcode/crabcode" "$HOME/.local/bin/crabcode"

crabcode --version
```

Windows 安装目录带版本号，更新时需要把用户 `PATH` 中旧的 `crabcode-*` 目录替换为新版本目录：

```powershell
$Version = "1.3.33"
$Package = "crabcode-$Version-win-x64"
$Zip = "$env:TEMP\$Package.zip"
$InstallRoot = "$env:LOCALAPPDATA\crabcode"
$Bin = Join-Path $InstallRoot $Package

Invoke-WebRequest `
  -Uri "https://github.com/acosmi/crabcode/releases/download/v$Version/$Package.zip" `
  -OutFile $Zip

Remove-Item $Bin -Recurse -Force -ErrorAction SilentlyContinue
New-Item -ItemType Directory -Force -Path $InstallRoot | Out-Null
Expand-Archive -Path $Zip -DestinationPath $InstallRoot -Force

$OldPath = [Environment]::GetEnvironmentVariable("Path", "User")
$Keep = ($OldPath -split ";") | Where-Object {
  $_ -and ($_ -notlike "$InstallRoot\crabcode-*")
}
[Environment]::SetEnvironmentVariable("Path", (($Keep + $Bin) -join ";"), "User")

& "$Bin\crabcode.exe" --version
```

更新前请退出正在运行的 CrabCode；Windows 更新后重新打开 PowerShell 让新的 `PATH` 生效。

### 校验文件完整性

在已下载发布包的目录中执行：

```bash
curl -fsSL -O https://github.com/acosmi/crabcode/releases/download/v1.3.33/checksums-sha256.txt
shasum -a 256 -c checksums-sha256.txt
```

Linux 也可以使用：

```bash
sha256sum -c checksums-sha256.txt
```

Windows 可下载 Windows 专用校验文件后对比：

```powershell
Invoke-WebRequest `
  -Uri "https://github.com/acosmi/crabcode/releases/download/v1.3.33/checksums-windows.txt" `
  -OutFile checksums-windows.txt

Get-Content checksums-windows.txt
Get-FileHash crabcode-1.3.33-win-x64.zip -Algorithm SHA256
```

### 快速上手

```bash
crabcode               # 进入交互式 TUI
crabcode -p "解释当前目录的代码结构"
crabcode --help
crabcode --version
```

第一次使用请在 TUI 中执行 `/login` 完成账号认证。

常用命令：

| 命令 | 用途 |
|---|---|
| `/login` | 登录或切换账号 |
| `/model` | 查看和切换可用模型 |
| `/clear` | 清空当前会话上下文 |
| `/history` | 查看历史会话 |
| `/permissions` | 查看或修改工具权限 |
| `/help` | 打开帮助 |

常用快捷键：

| 操作 | 按键 |
|---|---|
| 切换思考档位 | `Tab` |
| 中断当前任务 | `Esc` |
| 翻阅历史 | `↑` / `↓` |
| 滚动屏幕 | `PgUp` / `PgDn` 或鼠标滚轮 |
| 退出 | 连续按两次 `Ctrl+C` |

### 数据与隐私

- 用户配置、认证状态、历史和本地记忆默认保存在 `~/.crabcode/`。
- 工具执行默认受权限控制；文件和 Shell 操作需要按目录授权。
- 模型请求经由 Acosmi 网关转发到上游模型服务；具体可用模型和额度以账号状态为准。

### 反馈与支持

- Bug / 建议: <https://github.com/acosmi/crabcode/issues>
- 最新发布: <https://github.com/acosmi/crabcode/releases/latest>
- 官方网站: <https://acosmi.com/zh>
- 账号和词元问题：登录账户后台，或在 CrabCode 中执行 `/help`。

---

## English

### Overview

**CrabCode** is a terminal-native AI coding assistant. It brings model access, code search, file editing, shell execution, MCP tools, and GitHub workflows into one command-line experience, so you can understand code, debug, refactor, generate tests, and automate engineering tasks from the project directory you are already in.

This public repository hosts stable release packages and user-facing documentation. The latest release is **v1.3.33**, published on **2026-05-08 UTC**.

### Current Release

| Platform | Architecture | Asset |
|---|---:|---|
| macOS Apple Silicon | arm64 | `crabcode-1.3.33-darwin-arm64.tar.gz` |
| macOS Intel | x64 | `crabcode-1.3.33-darwin-x64.tar.gz` |
| Linux | arm64 | `crabcode-1.3.33-linux-arm64.tar.gz` |
| Linux | x64 | `crabcode-1.3.33-linux-x64.tar.gz` |
| Windows | x64 | `crabcode-1.3.33-win-x64.zip` |

All packages are available on the [latest release](https://github.com/acosmi/crabcode/releases/latest) page with SHA-256 checksum files.

### Core Features

- **Terminal-native TUI**: fullscreen interaction, streaming output, session history, themes, and English / Chinese UI.
- **Dynamic model routing**: available models, capabilities, and pricing are provided dynamically by the Acosmi SDK; use `/model` in the client to inspect and switch.
- **Engineering tools**: read, write, exact edit, full-text search, Glob matching, sandboxed Shell, and Notebook read/write.
- **MCP ecosystem**: configure and call Model Context Protocol servers for browser automation, external data, and custom tools.
- **Workflow support**: Hooks, Skills, Plan Mode, permissions, Git / GitHub collaboration, and cross-session memory.
- **Scheduled tasks**: `crabcode cron` uses an independent Rust scheduler daemon on macOS, Linux, and Windows.

### Architecture Status

CrabCode currently uses a **Rust core + TypeScript product layer**:

| Component | Role |
|---|---|
| `crabcode` | Rust CLI launcher that parses high-frequency flags and starts the interactive client |
| `dist/index.js` | TypeScript layer for TUI, agent loop, model SDK, MCP, and tool orchestration |
| `crabcode-cron` | Rust scheduled-task daemon; Unix socket on macOS/Linux, Named Pipe on Windows |
| `dist/vendor/ripgrep` | Bundled ripgrep binary for fast code search |

The historical Hub / Go daemon path has been retired; model and account capabilities go through `@acosmi/sdk-ts` and the Acosmi gateway.

### Installation

#### macOS

Choose your platform:

- Apple Silicon: `darwin-arm64`
- Intel: `darwin-x64`

```bash
VERSION=1.3.33
PLATFORM=darwin-arm64

curl -fsSL -o /tmp/crabcode.tar.gz \
  "https://github.com/acosmi/crabcode/releases/download/v${VERSION}/crabcode-${VERSION}-${PLATFORM}.tar.gz"

rm -rf "$HOME/.local/share/crabcode"
mkdir -p "$HOME/.local/share/crabcode" "$HOME/.local/bin"
tar -xzf /tmp/crabcode.tar.gz --strip-components=1 -C "$HOME/.local/share/crabcode"

xattr -dr com.apple.quarantine "$HOME/.local/share/crabcode" 2>/dev/null || true
ln -sf "$HOME/.local/share/crabcode/crabcode" "$HOME/.local/bin/crabcode"

crabcode --version
crabcode
```

If `~/.local/bin` is not in `PATH`, add this to `~/.zshrc` or `~/.bashrc`, then reopen your terminal:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

#### Linux

Choose your platform:

- x64: `linux-x64`
- arm64: `linux-arm64`

```bash
VERSION=1.3.33
PLATFORM=linux-x64

curl -fsSL -o /tmp/crabcode.tar.gz \
  "https://github.com/acosmi/crabcode/releases/download/v${VERSION}/crabcode-${VERSION}-${PLATFORM}.tar.gz"

rm -rf "$HOME/.local/share/crabcode"
mkdir -p "$HOME/.local/share/crabcode" "$HOME/.local/bin"
tar -xzf /tmp/crabcode.tar.gz --strip-components=1 -C "$HOME/.local/share/crabcode"
if command -v xattr >/dev/null 2>&1; then
  xattr -dr com.apple.quarantine "$HOME/.local/share/crabcode" 2>/dev/null || true
fi
ln -sf "$HOME/.local/share/crabcode/crabcode" "$HOME/.local/bin/crabcode"

crabcode --version
crabcode
```

#### Windows

Run in PowerShell:

```powershell
$Version = "1.3.33"
$Package = "crabcode-$Version-win-x64"
$Zip = "$env:TEMP\$Package.zip"
$InstallRoot = "$env:LOCALAPPDATA\crabcode"
$Bin = Join-Path $InstallRoot $Package

Invoke-WebRequest `
  -Uri "https://github.com/acosmi/crabcode/releases/download/v$Version/$Package.zip" `
  -OutFile $Zip

Remove-Item $Bin -Recurse -Force -ErrorAction SilentlyContinue
New-Item -ItemType Directory -Force -Path $InstallRoot | Out-Null
Expand-Archive -Path $Zip -DestinationPath $InstallRoot -Force

$CurrentPath = [Environment]::GetEnvironmentVariable("Path", "User")
if (($CurrentPath -split ";") -notcontains $Bin) {
  [Environment]::SetEnvironmentVariable("Path", "$CurrentPath;$Bin", "User")
}

& "$Bin\crabcode.exe" --version
```

Reopen PowerShell, then run:

```powershell
crabcode
```

#### Update an Existing Installation

An update only replaces application files. It does not remove local config, auth state, history, or memory data. Do not delete `~/.crabcode/`.

On macOS / Linux, reinstall into the same application directory to overwrite the old version:

```bash
VERSION=1.3.33
PLATFORM=darwin-arm64  # Use darwin-x64 for Intel macOS; linux-x64 or linux-arm64 for Linux

curl -fsSL -o /tmp/crabcode.tar.gz \
  "https://github.com/acosmi/crabcode/releases/download/v${VERSION}/crabcode-${VERSION}-${PLATFORM}.tar.gz"

rm -rf "$HOME/.local/share/crabcode"
mkdir -p "$HOME/.local/share/crabcode" "$HOME/.local/bin"
tar -xzf /tmp/crabcode.tar.gz --strip-components=1 -C "$HOME/.local/share/crabcode"
if command -v xattr >/dev/null 2>&1; then
  xattr -dr com.apple.quarantine "$HOME/.local/share/crabcode" 2>/dev/null || true
fi
ln -sf "$HOME/.local/share/crabcode/crabcode" "$HOME/.local/bin/crabcode"

crabcode --version
```

On Windows, the install directory includes the version number, so update the user `PATH` to replace old `crabcode-*` directories with the new one:

```powershell
$Version = "1.3.33"
$Package = "crabcode-$Version-win-x64"
$Zip = "$env:TEMP\$Package.zip"
$InstallRoot = "$env:LOCALAPPDATA\crabcode"
$Bin = Join-Path $InstallRoot $Package

Invoke-WebRequest `
  -Uri "https://github.com/acosmi/crabcode/releases/download/v$Version/$Package.zip" `
  -OutFile $Zip

Remove-Item $Bin -Recurse -Force -ErrorAction SilentlyContinue
New-Item -ItemType Directory -Force -Path $InstallRoot | Out-Null
Expand-Archive -Path $Zip -DestinationPath $InstallRoot -Force

$OldPath = [Environment]::GetEnvironmentVariable("Path", "User")
$Keep = ($OldPath -split ";") | Where-Object {
  $_ -and ($_ -notlike "$InstallRoot\crabcode-*")
}
[Environment]::SetEnvironmentVariable("Path", (($Keep + $Bin) -join ";"), "User")

& "$Bin\crabcode.exe" --version
```

Exit running CrabCode sessions before updating. On Windows, reopen PowerShell after the update so the new `PATH` takes effect.

### Verify File Integrity

Run this in the directory where you downloaded the release packages:

```bash
curl -fsSL -O https://github.com/acosmi/crabcode/releases/download/v1.3.33/checksums-sha256.txt
shasum -a 256 -c checksums-sha256.txt
```

On Linux you can also use:

```bash
sha256sum -c checksums-sha256.txt
```

For Windows:

```powershell
Invoke-WebRequest `
  -Uri "https://github.com/acosmi/crabcode/releases/download/v1.3.33/checksums-windows.txt" `
  -OutFile checksums-windows.txt

Get-Content checksums-windows.txt
Get-FileHash crabcode-1.3.33-win-x64.zip -Algorithm SHA256
```

### Quick Start

```bash
crabcode               # Interactive TUI
crabcode -p "Explain this repository"
crabcode --help
crabcode --version
```

On first use, run `/login` inside the TUI to authenticate your account.

Common slash commands:

| Command | Purpose |
|---|---|
| `/login` | Sign in or switch account |
| `/model` | Inspect and switch available models |
| `/clear` | Clear current session context |
| `/history` | View past sessions |
| `/permissions` | View or change tool permissions |
| `/help` | Open help |

Common shortcuts:

| Action | Key |
|---|---|
| Cycle thinking level | `Tab` |
| Interrupt current task | `Esc` |
| History up/down | `↑` / `↓` |
| Scroll screen | `PgUp` / `PgDn` or mouse wheel |
| Quit | `Ctrl+C` twice |

### Data & Privacy

- User configuration, auth state, history, and local memory are stored under `~/.crabcode/` by default.
- Tool execution is permission-gated by default; file and Shell access are authorized per directory.
- Model requests are routed through the Acosmi gateway to upstream model services; available models and quota depend on account status.

### Feedback & Support

- Bugs / suggestions: <https://github.com/acosmi/crabcode/issues>
- Latest release: <https://github.com/acosmi/crabcode/releases/latest>
- Official site: <https://acosmi.com/zh>
- Account and token issues: use your account dashboard, or run `/help` inside CrabCode.

---

<div align="center">

**CrabCode · Crafted by Acosmi · 2026**

</div>
