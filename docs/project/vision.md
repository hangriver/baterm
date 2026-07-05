# Baterm — Product Vision

> **Project codename**: `baterm`
> **Self-description (full, 6 words)**:
> **B**lock-managed **A**I-empowered **T**ree-view **E**mulator / **R**untime **M**anager
> **Self-description (short, 3 words)**:
> **B**lock-managed **A**I-empowered **T**erminal

---

## 1. Product Overview

Baterm is a lightweight, cross-platform terminal emulator built on **Tauri 2 + Rust**. Its core positioning is the **infrastructure-management and AI-empowered command cockpit**: a three-pane workspace that unifies SSH/Docker asset management, atomic Block-based command streams, AI-assisted error diagnosis, and Coding-Agent hosting into a single coherent working surface.

The product deliberately rejects the "terminal becomes IDE" path. The design philosophy is **"the terminal does not overstep"** — Baterm does not rebuild a full IDE; it focuses on doing the command-cockpit job extremely well, then layers AI as an accelerator rather than a replacement.

---

## 2. Core Values

| Value | Meaning |
|-------|---------|
| **Infrastructure assets + command cockpit unified** | Left sidebar exposes SSH hosts and Docker containers; center Block terminal; one window covers the entire ops workflow. |
| **AI accelerates; AI does not override** | `#` triggers natural-language → command translation; error blocks expose one-click Ask AI; Coding Agents run in dedicated isolated tabs. AI is the copilot, never the driver. |
| **Minimal kernel + tiny footprint + zero cloud lock-in** | Tauri 2 + Rust + ~7 MB install + BYOK + WebDAV sync. Functional parity with Warp without the cloud dependency. |

---

## 3. Layout: Four-Pane Workspace

The workspace is composed of four logical regions:

- **Top Tab Bar**: CLI tabs / Coding-Agent tabs / Editor tabs / Preview tabs / AI-Chat tabs. All tab types are siblings — none are popups.
- **Left Asset Tree Panel**: file tree, SSH manager, Docker console.
- **Center Main Stage**: actual rendering area (xterm.js / CodeMirror 6 / read-only renderer / chat stream).
- **Bottom Status Bar**: cwd, Shell type, Block counter, connection state.

A **Workspace** is a logical container for a group of tabs. Switching Workspaces swaps the entire tab group, the asset tree, and the SSH/Docker list as one atomic operation.

### 3.1 Main Stage 1: split & tile-able execution layer

Main Stage 1 is the **only** region that supports splitting and tiling. Each tile carries its own PTY process and is fully independent — one tile running a local shell can sit beside another tile SSH'd into prod-1. The standard ops scenario "local one side, SSH the other side" is the first-class use case.

- **Bidirectional splitting**: each tile can be split horizontally or vertically; the structure forms an arbitrary tree (consistent with iTerm2 / WezTerm).
- **Independent PTY per tile**: every tile owns its active Block and its PTY process; freeze + virtualize rules apply per tile.
- **Quick keys (proposed)**: `Cmd+D` horizontal split, `Cmd+Shift+D` vertical split, `Alt+HJKL` focus movement, `Cmd+W` close tile.

### 3.2 Main Stage 2: natural-language command line

Main Stage 2 is **not** a split product of Main Stage 1. It is a **fixed, single-container** natural-language input bar that sits below Main Stage 1. Its job is exactly one thing: translate natural language into an exact CLI command, then route that command to Main Stage 1's currently focused tile.

The two-step execution flow:

1. User types a natural-language request in Main Stage 2 (e.g., "list all `.txt` files in the current folder").
2. AI returns the exact command (e.g., `ls *.txt`).
3. User presses `Enter` in Main Stage 2 → the command is sent to Main Stage 1's currently focused tile as a new Block, **not yet executed**.
4. User presses `Enter` in the tile → the command actually runs.

This two-step Enter flow is a deliberate double-confirmation to prevent unauthorized execution. Main Stage 2 always routes to the currently focused tile; users switch focus with `Alt+HJKL`.

### 3.3 Right Sidebar: Chat Copilot (deep-dive)

For long-running debugging and principle-oriented discussions, a separate **Chat Copilot** lives in the right sidebar. Unlike the one-shot Main Stage 2 interactions, Copilot sessions are **persistent, named, and context-anchored**.

- A user-selected Block or file becomes a context anchor (`@b023` references a Block, `@nginx.conf:42-58` references a file range).
- Each session has a name (e.g., `prod-nginx-debug`) and is persisted per Workspace.
- AI-suggested commands require explicit user approval before execution.

---

## 4. AI Integration: Five Modes

Baterm treats AI as five distinct entry points, each tuned for a different cognitive task:

| # | Mode | Entry | Context | Persistence |
|---|------|-------|---------|-------------|
| 1 | **Passive diagnosis** | "Ask AI" button on a failed Block | The single Block's command + output + exit code | No |
| 2 | **Active completion** | `#` prefix in the active input | Current Profile (Shell / OS / cwd) | No |
| 3 | **Agent hosting** | "Claude Code Here" button | Workspace root directory | Yes (as tab) |
| 4 | **One-line dialogue** (Main Stage 2) | `Cmd+B` toggle | Current focused tile's Profile | No |
| 5 | **Copilot deep-dive** (Right sidebar) | Always-visible sidebar | Anchored Block / file / file range | Yes (named sessions) |

The dual-AI design (Main Stage 2 + Copilot) is a v0.5+ correction. Previously Baterm had only one-shot AI; v0.5+ adds the persistent, context-anchored Copilot mode for scenarios where users want depth, not speed.

---

## 5. Technical Pillars

### 5.1 Baseline metrics

- **Install footprint**: ~7 MB total (Tauri 2 strips Chromium and delegates to OS WebView).
- **Cold start**: millisecond-class (Rust PTY kernel + system WebView).
- **Privacy**: zero mandatory cloud account; full **BYOK** (Bring Your Own Key); WebDAV-based decentralized sync for snippets and configuration.

### 5.2 Two engineering breakthroughs

**Breakthrough 1 — Atomic Block lifecycle (Freeze & Virtualize)**. Every command is a discrete Block. At any moment, only the bottom Block holds a live `xterm.js` instance; finished Blocks serialize to static HTML via `@xterm/addon-serialize` and the underlying instance is disposed. A virtual-scroll layer keeps memory flat regardless of how many Block history entries exist.

**Breakthrough 2 — Alternate-screen-buffer awareness (per-tile)**. Rust-side parsing of PTY byte streams detects `CSI ? 1049 h` (alternate-screen enter) and `CSI ? 1049 l` (alternate-screen leave) sequences on a per-tile basis. When triggered, a full-screen overlay covers only that tile — not the entire workspace — so multi-tile concurrency is preserved while full-screen TUI apps (`vim`, `htop`, `less`) still get a clean takeover.

### 5.3 CodeMirror 6 (not Monaco)

The editor uses **CodeMirror 6** in tab form (not Monaco, not a popup). Rationale: ~200–400 KB bundle (vs Monaco's ~5 MB) makes the 7 MB target feasible; Vim mode is more natural; the AI inline-autocomplete extension ecosystem is more mature; million-line files open in seconds. Terax made the same choice; Baterm uses the same editor at the same position in the stack, but Baterm's editor is one of many tab types — "assistant" rather than "primary".

---

## 6. Non-Goals

Baterm deliberately does **not** build:

- A full IDE (no Git integration, no Web Preview, no language-server-first design).
- A GUI file manager (the asset tree is a navigation surface, not a manager).
- A remote collaboration / shared Block model (conflicts with "local-first").
- A forced cloud account (privacy is the floor).
- An integrated multiplexer (tmux / WezTerm mux already exist; no need to rebuild).

The product's discipline is **restraint**: resist feature creep, do the command cockpit exceptionally well.

---

## 7. Naming Convention

- **Codename**: `baterm` (lowercase, six letters)
- **Project display name**: `Baterm` (capitalized)
- **CLI command**: `baterm` (lowercase, matching Docker / kubectl conventions)
- **Config directory**: `~/.config/baterm/`
- **Repository**: https://github.com/hangriver/baterm

The six letters encode six core capabilities (full description above). The short 3-word form (`Block-managed AI-empowered Terminal`) is reserved for elevator pitches, README headlines, and external communication. Both versions are valid self-descriptions of the same product.

---

## 8. Why Baterm Will Win

Baterm's defensible advantages over the existing field:

1. **Asset panel + atomic Blocks**: SSH / Docker lists integrated with a Block-based command stream — none of Warp, Wave Terminal, or Terax does this. The combination is the unique position.
2. **Multi-Workspace full-stack isolation**: tab group + SSH list + Docker list + asset tree all switch atomically. Terax is single-workspace; Warp doesn't isolate. This is a rare capability.
3. **AI that does not override**: BYOK + WebDAV + per-call approval gating. Main Stage 2 routes to focused tile; Copilot requires explicit approval. Compare against Warp's subscription-only cloud AI.
4. **"The terminal does not overstep"**: as every other AI terminal expands into IDE territory (Terax includes Git, Web Preview, full CodeMirror workspace), Baterm holds the line on "command cockpit only" — a deliberate counter-positioning.

---

## 9. Status

- **Current stage**: documentation baseline complete (v0.5.2, 2026-07-05); entering implementation.
- **Next milestone**: skeleton demo — Tauri 2 + Rust + portable-pty + xterm.js rendering 100 Blocks under 30 MB memory.
- **Target MVP**: see [roadmap](../product/roadmap.md) (to be written).

---

> This document is the project's source of truth for "what Baterm is and why it exists". When implementation details change, update [architecture.md](./architecture.md) instead. When design decisions are made, append a new entry in [../decisions/](../decisions/) referencing back to this document.