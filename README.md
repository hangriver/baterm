# Baterm

> **Block-managed AI-empowered Terminal**
> *A lightweight, AI-empowered terminal cockpit built on Tauri 2 + Rust. Infrastructure assets, atomic Block command streams, and AI assistance unified in one workspace.*

[中文版说明](#中文版说明) ↓

---

## What is Baterm?

Baterm is a **lightweight, cross-platform terminal emulator** that fuses three things into one workspace:

1. **Infrastructure assets** — SSH host list and Docker container list, always one click away.
2. **Atomic Block command streams** — every command is a discrete, freeze-able Block; thousands of history entries fit in constant memory.
3. **AI assistance that does not override** — natural-language input, error-block diagnosis, persistent Chat Copilot, Coding Agent hosting. All BYOK, all local-first.

It is built on **Tauri 2 + Rust**, ships as a **~7 MB** binary, and runs natively on macOS / Windows / Linux. It deliberately does **not** try to become an IDE.

The full project name: **B**lock-managed **A**I-empowered **T**ree-view **E**mulator / **R**untime **M**anager. The short form is reserved for elevator pitches.

---

## Features

### Asset-panel + terminal cockpit, unified
The left sidebar shows SSH hosts and Docker containers; one click opens a new terminal tab connected to that asset. **The DevOps workflow stops requiring four open windows.**

### Main Stage 1: free tile & split
Split the terminal region horizontally or vertically into as many tiles as you need. Each tile holds its own PTY process — run a local shell in one tile, SSH into prod-1 in another, watch logs in a third.

### Main Stage 2: natural-language command line
Type what you mean in plain English (`"list every .txt file in the current folder"`); Baterm returns the exact shell command (`ls *.txt`); press `Enter` to send it to the currently focused tile; press `Enter` again in the tile to execute. **Two-step confirmation prevents accidental execution.**

### Right-sidebar Chat Copilot
For longer debugging sessions: select a Block or file as the context anchor, then have a multi-round conversation with the AI. Sessions are named, persisted per Workspace, and require explicit approval before the AI's suggested commands execute.

### Coding Agent hosting
Run Claude Code / Codex / Aider in a dedicated isolated tab. The terminal provides the high-performance host; the Agent handles its own file/test/Git workflow.

### BYOK + WebDAV
Bring your own API key for every supported LLM provider. Snippets and configuration sync via WebDAV — no central server, no mandatory account.

---

## Quick start

> Implementation is in early stages (Tauri 2 + Rust). The skeleton demo is the first milestone.

```bash
# Clone
git clone https://github.com/hangriver/baterm.git
cd baterm

# Read the docs (mandatory before any contribution)
cat docs/README.md
cat docs/project/vision.md
cat docs/guidelines/agent-prompt.md
```

A runnable binary is not yet released. Track progress in [`docs/product/roadmap.md`](docs/product/roadmap.md).

---

## Documentation

Everything lives under [`docs/`](docs/README.md). The two most important documents for a first-time reader:

- **[`docs/project/vision.md`](docs/project/vision.md)** — what Baterm is, who it is for, what it explicitly is not
- **[`docs/guidelines/agent-prompt.md`](docs/guidelines/agent-prompt.md)** — behavioral rules for AI agents working on this codebase

For deeper context, see [`docs/README.md`](docs/README.md) for the full navigation map.

---

## Roadmap (high-level)

| Phase | Target | Status |
|-------|--------|--------|
| **v0.1 — Skeleton demo** | Tauri 2 + Rust + portable-pty + xterm.js rendering 100 Blocks under 30 MB | Planned |
| **v0.2 — Asset panel** | SSH / Docker list integration + one-click connection | Planned |
| **v0.3 — Main Stage 1 free tile** | Horizontal + vertical split with focus routing | Planned |
| **v0.4 — Main Stage 2 + Copilot** | Natural-language command line + Chat Copilot sidebar | Planned |
| **v1.0 — MVP** | All five AI modes, multi-Workspace isolation, WebDAV sync, BYOK | Planned |

See [`docs/product/roadmap.md`](docs/product/roadmap.md) for the full version plan.

---

## Contributing

Issues and PRs are welcome, but **read [`docs/README.md`](docs/README.md) first**. The docs are the contract: if you change code in a way that contradicts a doc, update the doc in the same PR.

Before opening a PR:

1. Skim [`docs/project/vision.md`](docs/project/vision.md) — make sure your change aligns with the project's stated non-goals.
2. Skim [`docs/guidelines/style-guide.md`](docs/guidelines/style-guide.md) and [`docs/guidelines/commit-convention.md`](docs/guidelines/commit-convention.md).
3. If your PR makes a significant architectural choice, append a new entry to [`docs/decisions/`](docs/decisions/) in the same PR.

---

## License

TBD — see [`docs/project/vision.md`](docs/project/vision.md) for the working assumption. Likely MIT, pending final decision.

---

## 中文版说明

### Baterm 是什么？

Baterm 是一款基于 **Tauri 2 + Rust** 的**轻量级、跨平台终端模拟器**。它把三件事整合到同一个工作区：

1. **基础设施资产** —— SSH 主机列表 + Docker 容器列表，一键直达
2. **原子化 Block 命令流** —— 每次命令是独立的、可冻结的 Block；上千条历史占用恒定内存
3. **AI 辅助但不 AI 越权** —— 自然语言输入、错误 Block 诊断、持久 Chat Copilot、Coding Agent 宿主；全部 BYOK，本地优先

包体积 **~7 MB**，原生支持 macOS / Windows / Linux。**不**尝试变成 IDE。

完整项目名：**B**lock-managed **A**I-empowered **T**ree-view **E**mulator / **R**untime **M**anager。简称用于电梯演讲等日常场景。

### 核心特性

- **资产面板 + 终端座舱一体化**：左侧栏展示 SSH 主机和 Docker 容器，一键新建终端 tab 连过去 —— DevOps 工作流不再需要开四个窗口
- **Main Stage 1：自由分块**：把终端区水平或垂直拆成多个 tile，每个 tile 独立跑 PTY 进程 —— 一边本地 shell、一边 SSH prod-1、一边看日志
- **Main Stage 2：自然语言命令行**：用中文或英文写下意图（"列出当前文件夹所有 .txt 文件"）；Baterm 返回精确命令（`ls *.txt`）；按 Enter 送到当前激活 tile；tile 里再按 Enter 才真正执行 —— **两步确认防误操作**
- **右侧 Chat Copilot**：长调试会话专用 —— 选中 Block 或文件作为上下文锚定，多轮对话，会话命名持久化，AI 给的命令需显式批准才执行
- **Coding Agent 宿主**：在隔离 tab 里跑 Claude Code / Codex / Aider，终端做高性能宿主
- **BYOK + WebDAV**：自带 API Key；配置和 Snippets 通过 WebDAV 同步 —— 无中央服务器、无强制账号

### 快速开始

> 项目还在早期阶段（Tauri 2 + Rust），骨架 demo 是第一个里程碑。

```bash
git clone https://github.com/hangriver/baterm.git
cd baterm

# 必读文档
cat docs/README.md
cat docs/project/vision.md
cat docs/guidelines/agent-prompt.md
```

可运行的二进制版本尚未发布。进度追踪见 [`docs/product/roadmap.md`](docs/product/roadmap.md)。

### 文档

全部文档位于 [`docs/`](docs/README.md)。首次阅读最重要的两份：

- **[`docs/project/vision.md`](docs/project/vision.md)** —— Baterm 是什么、为谁服务、明确**不**做什么
- **[`docs/guidelines/agent-prompt.md`](docs/guidelines/agent-prompt.md)** —— AI 代理在本项目工作的行为规则

完整导航见 [`docs/README.md`](docs/README.md)。

### 路线图（高层）

| 阶段 | 目标 | 状态 |
|------|------|------|
| **v0.1 — 骨架 demo** | Tauri 2 + Rust + portable-pty + xterm.js 渲染 100 个 Block 占用 <30 MB | 计划中 |
| **v0.2 — 资产面板** | SSH / Docker 列表集成 + 一键连接 | 计划中 |
| **v0.3 — Main Stage 1 free tile** | 水平 + 垂直分块 + 焦点路由 | 计划中 |
| **v0.4 — Main Stage 2 + Copilot** | 自然语言命令行 + Chat Copilot 侧栏 | 计划中 |
| **v1.0 — MVP** | 全部 5 种 AI 模式 + 多 Workspace 隔离 + WebDAV 同步 + BYOK | 计划中 |

完整版本规划见 [`docs/product/roadmap.md`](docs/product/roadmap.md)。

### 贡献

欢迎 Issue 和 PR，但**请先读 [`docs/README.md`](docs/README.md)**。文档即契约：你改代码时如果跟某份文档矛盾，请在同一个 PR 里更新那份文档。

提交 PR 前：

1. 先看 [`docs/project/vision.md`](docs/project/vision.md) —— 确认你的改动符合项目的非目标
2. 看 [`docs/guidelines/style-guide.md`](docs/guidelines/style-guide.md) 和 [`docs/guidelines/commit-convention.md`](docs/guidelines/commit-convention.md)
3. 如果你的 PR 涉及重大架构选择，请在同一个 PR 里往 [`docs/decisions/`](docs/decisions/) 加一个新条目

### 许可证

待定 —— 见 [`docs/project/vision.md`](docs/project/vision.md) 的工作假设。倾向 MIT，待最终决定。