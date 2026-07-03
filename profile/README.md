# spec-graph

**Declaration engine — brain, not hands**

[![npm version](https://img.shields.io/npm/v/spec-graph.svg)](https://www.npmjs.com/package/spec-graph)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/node-%3E%3D18-brightgreen.svg)](https://nodejs.org)

## 什么是 spec-graph？

spec-graph v3 是一个**声明引擎**（declaration engine）：

```
spec-graph 是大脑，不是双手
─────────────────────────────
✓ 生成 dispatch manifest（9-section envelope）
✓ 评估产出物通过严格质量门
✓ 追踪 9-stage FSM 状态
✗ 永不直接调用 agent
✗ 永不产生子进程
✗ 永不写代码或文档

所有 agent 执行委托给外部协调者
（Claude Code skills, CI/CD, 自定义编排器）
```

它管理一个 9 阶段状态机，为 sub-agent 生成 dispatch manifest，并通过严格质量门评估产出物。spec-graph 不执行任何实际操作 — 它是开发流程的"大脑"。

## 架构

```
┌─────────────────────────────────────────────────┐
│  Skills (SKILL.md)  ← AI agent 编排层          │
│  packages/skills/    11 个 entry skills        │
├─────────────────────────────────────────────────┤
│  CLI (Node.js)       ← 人类/agent 接口          │
│  packages/cli/       22 个原子命令             │
├─────────────────────────────────────────────────┤
│  Core (TypeScript)   ← 声明引擎                 │
│  packages/core/      12 个模块                 │
│  automator / planning / gate-enforcement       │
│  dispatch / knowledge-base / composer / ...    │
└─────────────────────────────────────────────────┘
```

## 仓库

| 仓库 | 说明 | 状态 |
|------|------|------|
| [monorepo](https://github.com/spec-graph/monorepo) | 主仓库：git submodule 组织所有 package | ✅ |
| [core](https://github.com/spec-graph/core) | 引擎：automator, gate-enforcement, dispatch, planning, knowledge-base | ✅ v3 |
| [cli](https://github.com/spec-graph/cli) | CLI：dispatch manifest generator + gate evaluator | ✅ v3 |
| [skills](https://github.com/spec-graph/skills) | 11 个 SKILL.md，用于 Claude Code 集成 | ✅ v3 |
| [server](https://github.com/spec-graph/server) | HTTP/WebSocket 服务器 | 🚧 |
| [ui](https://github.com/spec-graph/ui) | Web UI (React SPA) 实时仪表盘 | 📋 |

## 核心能力

- **9-Stage FSM**: specify → specs → design → tasks → implement → review → test → accept → integrate
- **Strict Quality Gates**: 每个阶段 entry/exit criteria 自动评估
- **Dispatch Manifests**: 9-section envelope JSON，供 sub-agent 消费
- **Parallel Execution**: implement 阶段多 capability 并行调度
- **Knowledge Base**: 内置方法论库，支持本地覆盖
- **Progressive Recovery**: 4-level 重试策略，diagnosis-driven
- **Session Persistence**: 基于文件的 state 持久化 (`.spec-graph/sessions/`)
- **Meeting Runtime**: 多 agent 协作讨论管理
- **Worktree Isolation**: 并行 sub-agent 的 git worktree 隔离
- **Hook Integration**: PostToolUse hook 自动注入 system-reminder

## 快速开始

```bash
# 全局安装 CLI
npm install -g spec-graph

# 初始化项目
spec-graph init

# 创建会话并确认计划
spec-graph plan "Build JWT authentication" --fallback --confirm

# 生成 dispatch manifest（外部协调者读取并调度 sub-agent）
spec-graph dispatch --session <id> --json

# 提交 agent 产出物进行 gate 评估
spec-graph submit --session <id> --result '<json>'

# 或使用 AI agent skill 自动化整个循环
# /spec-graph-auto "Build JWT authentication"
```

## AI Agent 集成

spec-graph 提供两层接口：

1. **Skills**（Claude Code）：`/spec-graph-plan`, `/spec-graph-auto`, `/spec-graph-status`, `/spec-graph-intervene`
2. **CLI**（任何 agent）：通过 shell 命令调用 `dispatch --json` / `submit --result`

## 与其他工具对比

| 特性 | spec-graph v3 | wdf | BMAD | spec-kit |
|------|-------------|-----|------|----------|
| 架构哲学 | brain-not-hands | executor | methodology | scaffolding |
| FSM 阶段 | 9 | 4 | — | 4 |
| 质量门 | 9 stage gates | 4 | — | — |
| 并行执行 | ✅ wave-based | ❌ | ❌ | ❌ |
| 多 Agent 协作 | ✅ meeting protocol | ❌ | ✅ | ❌ |
| Brownfield 支持 | ✅ sense module | — | — | — |
| Agent 无关 | ✅ Claude Code + Codex + CI | — | ✅ | — |

## 文档

- [架构文档](https://github.com/spec-graph/monorepo/blob/main/docs/ARCHITECTURE.md)
- [Agent 集成指南](https://github.com/spec-graph/monorepo/blob/main/docs/agent-integration-guide.md)
- [v3.0 迁移指南](https://github.com/spec-graph/monorepo/blob/main/docs/migration-3.0.md)

## 贡献

欢迎贡献！MIT 许可证。

## 链接

- **npm**: https://www.npmjs.com/package/spec-graph
- **Issues**: https://github.com/spec-graph/monorepo/issues
- **Discussions**: https://github.com/orgs/spec-graph/discussions
