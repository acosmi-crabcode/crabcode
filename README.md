<div align="center">

# 🦀 CrabCode · 蟹码

**终端原生的 AI 编程助手 · 多模型 · 多工具 · 多场景**
**Terminal-native AI coding assistant · Multi-model · Multi-tool · Multi-context**

[![Latest Release](https://img.shields.io/github/v/release/acosmi-crabcode/crabcode?display_name=tag&label=latest)](https://github.com/acosmi-crabcode/crabcode/releases)
[![Platform](https://img.shields.io/badge/platform-macOS--arm64-blue)](#-安装--installation)
[![Status](https://img.shields.io/badge/status-active-success)]()
[![Issues](https://img.shields.io/github/issues/acosmi-crabcode/crabcode?label=issues)](https://github.com/acosmi-crabcode/crabcode/issues)

[简体中文](#-简介) · [English](#-overview)

</div>

---

## 🎁 限时福利 · 5 月 31 日前免费领词元（Token） / Free Tokens Until May 31

> **🇨🇳 新用户注册即送词元（tk）福利包**，覆盖日常编码、对话、工具调用、思考全场景。
> - 🌐 注册地址：<https://acosmi.com/zh>
> - 🗓️ 活动截止：**2026 年 5 月 31 日**
> - 💡 注册完成后在 CrabCode 中执行 `/login` 即开通词元额度
> - 📈 老用户邀请新用户额外赠送词元
>
> **🇬🇧 New users get a free token (tk) bundle on signup**, covering coding, chat, tool calls, and thinking modes.
> - 🌐 Sign up at <https://acosmi.com/zh>
> - 🗓️ Promotion ends: **May 31, 2026**
> - 💡 After signup, run `/login` inside CrabCode to activate your quota
> - 📈 Referring new users earns bonus tokens

---

# 🇨🇳 简体中文

## 📖 简介

**CrabCode（蟹码）** 是一款运行在终端的 AI 编程助手。它把主流大模型、代码工具、MCP 生态、IDE 联动整合进一个统一的命令行界面，专为开发者的真实工程场景设计。

无需切换窗口、无需复制粘贴，从代码理解、bug 定位、批量重构到测试生成，所有工作都在 shell 里完成。

## ✨ 核心特性

### 🤖 多模型 · 多档思考

| 模型 | 用途 |
|---|---|
| **DeepSeek v4 Flash** | 默认日常对话 / 代码生成（响应快、性价比高） |
| **DeepSeek v4 Pro** | 进阶推理 / 复杂代码任务 |
| **智谱 GLM-5.1** | 中文场景 / 多模态理解 |
| **Kimi 2.6（256K）** | 超长上下文（最大 256K tokens） |
| **通义千问 3.6 Plus** | 默认 fallback，国产替代路径 |
| **Claude Opus / Sonnet / Haiku 系列** | 复杂推理 / 长上下文场景 |

支持 **3 档思考模式**：

- 🟢 **关闭思考（Off）**——直接输出，适合简单问答
- 🟡 **标准思考（High）**——平衡推理深度与速度
- 🔴 **深度思考（Max）**——最大推理强度，适合架构设计、复杂排错

`Tab` 键即可在档位间循环。

### 🛠️ 完备的代码工具集

- **文件操作**：Read / Write / Edit（精确字串替换 + 整文件改写）
- **代码检索**：Glob 模式匹配 · Grep 内容搜索 · 路径感知的精准定位
- **Shell 执行**：沙盒化 Bash，权限可控，背景任务支持
- **任务编排**：多任务追踪、子智能体并行、计划模式
- **Notebook**：Jupyter ipynb 完整读写

### 🌐 联网与外部数据

- 内置 **WebSearch** / **WebFetch** 工具
- 网页内容自动提取 + 摘要
- 实时检索保证信息新鲜度

### 🔌 MCP 生态

完整兼容 [Model Context Protocol](https://modelcontextprotocol.io)：

- 🌐 浏览器自动化（页面交互 / 截图 / 表单填写）
- 📧 Gmail / Calendar / Drive 等 Google 套件
- 🗂️ 自定义 MCP server 零成本接入

### 💼 工作流增强

- **钩子（Hooks）**：在用户消息提交、工具调用前后注入自定义逻辑
- **技能（Skills）**：项目级 / 用户级可调用的能力包
- **跨会话记忆**：项目偏好、用户档案、反馈、外部参考四类持久化
- **Git / GitHub** 集成：commit · PR 创建 · issue 管理一气呵成
- **Plan Mode**：先策划后执行，避免破坏性误操作
- **定时任务**：crabcode-cron 守护进程支持周期性任务调度

### 🎨 终端原生体验

- 全屏 TUI 渲染，支持鼠标滚轮 / 键盘翻页 / tmux 集成
- 流式 token 输出，思考过程可视化
- 中 / 英双语界面，可切换主题

## 🏗️ 系统组成（顶层视图）

CrabCode 由以下组件协作运行：

| 组件 | 角色 |
|---|---|
| **CLI 入口** | 启动会话、解析命令、调度后端守护进程 |
| **Hub 守护进程** | 模型路由、工具调用、流式响应、MCP 协议桥接 |
| **Cron 守护进程** | 定时任务调度（独立守护，故障域隔离） |
| **TUI 前端** | 终端渲染、流式显示、用户输入处理 |

各组件以本地 socket 通信，独立守护、独立升级，故障域隔离。

## 🚀 安装

### macOS (Apple Silicon · M1/M2/M3/M4)

```bash
# 1. 下载最新版本
curl -fsSL -o /tmp/crabcode.tar.gz \
  https://github.com/acosmi-crabcode/crabcode/releases/latest/download/crabcode-v1.3.23-darwin-arm64.tar.gz

# 2. 解压到本地
mkdir -p ~/.local/share/crabcode
tar -xzf /tmp/crabcode.tar.gz --strip-components=1 -C ~/.local/share/crabcode

# 3. 去除 Gatekeeper 隔离（macOS 必需，否则首次启动会被拦下）
xattr -dr com.apple.quarantine ~/.local/share/crabcode/

# 4. 加入 PATH
mkdir -p ~/.local/bin
ln -sf ~/.local/share/crabcode/crabcode ~/.local/bin/crabcode

# 如果 ~/.local/bin 不在 PATH，把这一行加进 ~/.zshrc 或 ~/.bashrc
# export PATH="$HOME/.local/bin:$PATH"

# 5. 启动
crabcode
```

### 校验文件完整性（可选）

```bash
curl -fsSL -O https://github.com/acosmi-crabcode/crabcode/releases/latest/download/checksums-sha256.txt
shasum -a 256 -c checksums-sha256.txt
```

### 其它平台

| 平台 | 状态 |
|---|---|
| macOS arm64 (Apple Silicon) | ✅ 已发布（v1.3.23） |
| macOS x64 (Intel) | 🚧 计划中 |
| Linux arm64 | 🚧 计划中 |
| Linux x64 | 🚧 计划中 |
| Windows x64 | 🚧 计划中 |

> 在 [Issues](https://github.com/acosmi-crabcode/crabcode/issues) 留言可投票优先级。

## ⚡ 快速上手

启动会话：

```bash
crabcode               # 进入交互式 TUI
crabcode --help        # 查看所有命令
crabcode --version     # 查看当前版本
```

第一次启动执行 `/login` 完成账号认证，同时领取免费词元额度。

### 常用斜杠命令

| 命令 | 用途 |
|---|---|
| `/login` | 账号登录 / 切换 |
| `/model` | 切换模型 |
| `/clear` | 清空当前会话上下文 |
| `/history` | 查看历史会话 |
| `/permissions` | 查看 / 修改工具权限 |
| `/help` | 帮助 |

### 常用快捷键

| 操作 | 按键 |
|---|---|
| 切换思考档位 | `Tab` |
| 中断当前任务 | `Esc` |
| 翻阅历史 | `↑` / `↓` |
| 滚动屏幕 | `PgUp` / `PgDn`（或鼠标滚轮） |
| 退出 | 连续按两次 `Ctrl+C` |

## 🐛 反馈与支持

- **Bug / 建议**：[https://github.com/acosmi-crabcode/crabcode/issues](https://github.com/acosmi-crabcode/crabcode/issues)
- **官方网站**：<https://acosmi.com/zh>
- **词元活动 / 账户问题**：登录账户后台或在 CrabCode 中执行 `/help`

## 🔒 隐私与本地化

- 所有用户数据（认证信息、历史、设置）保存在本机 `~/.crabcode/`
- 模型对话经由 Acosmi 网关转发到上游模型供应商，不留存对话内容
- 工具执行（Bash / 文件操作）默认沙盒化，可按目录粒度授权

---

# 🇬🇧 English

## 📖 Overview

**CrabCode** is a terminal-native AI coding assistant. It unifies leading large language models, a complete code-tool suite, the MCP ecosystem, and IDE integration into a single command-line interface — purpose-built for real engineering workflows.

No window switching, no copy-pasting. From code understanding and bug localization to batch refactors and test generation, everything happens in your shell.

## ✨ Core Features

### 🤖 Multi-Model · Multi-Level Thinking

| Model | Use case |
|---|---|
| **DeepSeek v4 Flash** | Default for daily coding & chat (fast, cost-effective) |
| **DeepSeek v4 Pro** | Advanced reasoning / complex code tasks |
| **Zhipu GLM-5.1** | Chinese-language / multimodal scenarios |
| **Kimi 2.6 (256K)** | Ultra-long context (up to 256K tokens) |
| **Qwen 3.6 Plus** | Default fallback, domestic alternative |
| **Claude Opus / Sonnet / Haiku** | Complex reasoning · long-context scenarios |

3 thinking levels:

- 🟢 **Off** — direct output, ideal for quick questions
- 🟡 **High** — balanced depth and speed
- 🔴 **Max** — maximum reasoning strength for architecture, deep debugging

Cycle with `Tab`.

### 🛠️ Complete Code Toolkit

- **File ops**: Read / Write / Edit (exact string replace + full rewrite)
- **Code search**: Glob pattern matching · Grep content search · path-aware navigation
- **Shell execution**: sandboxed Bash with permission control and background task support
- **Task orchestration**: multi-task tracking, parallel sub-agents, plan mode
- **Notebook**: full Jupyter ipynb read/write

### 🌐 Web & External Data

- Built-in **WebSearch** / **WebFetch**
- Automatic web content extraction + summarization
- Real-time search for fresh information

### 🔌 MCP Ecosystem

Full [Model Context Protocol](https://modelcontextprotocol.io) compatibility:

- 🌐 Browser automation (page interaction / screenshots / form filling)
- 📧 Gmail / Calendar / Drive and other Google suite
- 🗂️ Zero-cost integration of custom MCP servers

### 💼 Workflow Enhancements

- **Hooks**: inject custom logic before/after user messages and tool calls
- **Skills**: project-scoped / user-scoped capability packs
- **Cross-session memory**: 4 categories — project, user, feedback, references
- **Git / GitHub integration**: commit · PR creation · issue management in one flow
- **Plan Mode**: plan first, execute later — avoid destructive mistakes
- **Scheduled tasks**: crabcode-cron daemon for periodic job scheduling

### 🎨 Terminal-Native Experience

- Fullscreen TUI with mouse / keyboard / tmux support
- Streaming token output with visible thinking
- Bilingual UI (EN/CN), themeable

## 🏗️ System Composition (Top-Level)

CrabCode runs as a coordinated set of components:

| Component | Role |
|---|---|
| **CLI entrypoint** | Session launch, command parsing, daemon orchestration |
| **Hub daemon** | Model routing, tool dispatch, streaming responses, MCP bridging |
| **Cron daemon** | Scheduled job runner (isolated process for fault containment) |
| **TUI frontend** | Terminal rendering, streaming display, user input |

All components communicate via local sockets — independently supervised, independently upgradable, with isolated failure domains.

## 🚀 Installation

### macOS (Apple Silicon · M1/M2/M3/M4)

```bash
# 1. Download the latest release
curl -fsSL -o /tmp/crabcode.tar.gz \
  https://github.com/acosmi-crabcode/crabcode/releases/latest/download/crabcode-v1.3.23-darwin-arm64.tar.gz

# 2. Extract to your local share dir
mkdir -p ~/.local/share/crabcode
tar -xzf /tmp/crabcode.tar.gz --strip-components=1 -C ~/.local/share/crabcode

# 3. Remove Gatekeeper quarantine attribute (required on macOS)
xattr -dr com.apple.quarantine ~/.local/share/crabcode/

# 4. Add to your PATH
mkdir -p ~/.local/bin
ln -sf ~/.local/share/crabcode/crabcode ~/.local/bin/crabcode

# Add this line to ~/.zshrc or ~/.bashrc if ~/.local/bin is not in PATH:
# export PATH="$HOME/.local/bin:$PATH"

# 5. Launch
crabcode
```

### Verify file integrity (optional)

```bash
curl -fsSL -O https://github.com/acosmi-crabcode/crabcode/releases/latest/download/checksums-sha256.txt
shasum -a 256 -c checksums-sha256.txt
```

### Other platforms

| Platform | Status |
|---|---|
| macOS arm64 (Apple Silicon) | ✅ Released (v1.3.23) |
| macOS x64 (Intel) | 🚧 Planned |
| Linux arm64 | 🚧 Planned |
| Linux x64 | 🚧 Planned |
| Windows x64 | 🚧 Planned |

> Comment on [Issues](https://github.com/acosmi-crabcode/crabcode/issues) to vote on priority.

## ⚡ Quick Start

```bash
crabcode               # Interactive TUI
crabcode --help        # All commands
crabcode --version     # Current version
```

Run `/login` on first use to authenticate and claim your free token quota.

### Common slash commands

| Command | Purpose |
|---|---|
| `/login` | Sign in / switch account |
| `/model` | Switch model |
| `/clear` | Clear current session context |
| `/history` | View past sessions |
| `/permissions` | View / modify tool permissions |
| `/help` | Help |

### Common shortcuts

| Action | Key |
|---|---|
| Cycle thinking level | `Tab` |
| Interrupt current task | `Esc` |
| History up/down | `↑` / `↓` |
| Scroll screen | `PgUp` / `PgDn` (or mouse wheel) |
| Quit | `Ctrl+C` twice |

## 🐛 Feedback & Support

- **Bug / suggestion**: [https://github.com/acosmi-crabcode/crabcode/issues](https://github.com/acosmi-crabcode/crabcode/issues)
- **Official site**: <https://acosmi.com/zh>
- **Token / account issues**: log into your account dashboard, or run `/help` inside CrabCode

## 🔒 Privacy & Local-First

- All user data (credentials, history, settings) stays on your machine in `~/.crabcode/`
- Model traffic is routed via the Acosmi gateway to upstream providers — **conversation content is not stored**
- Tool execution (Bash / file ops) is sandboxed by default, with per-directory authorization

---

## 📜 Version

Latest release: **v1.3.23** (released 2026-04-28)

Full changelog at [Releases](https://github.com/acosmi-crabcode/crabcode/releases).

---

<div align="center">

**🦀 Crafted by Acosmi · 2026**

</div>
