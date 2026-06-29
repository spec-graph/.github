# spec-graph

**领域中立的规格驱动工作流编排内核**

[![npm version](https://img.shields.io/npm/v/@spec-graph/core.svg)](https://www.npmjs.com/package/@spec-graph/core)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/node-%3E%3D18-brightgreen.svg)](https://nodejs.org)

## 什么是 spec-graph？

spec-graph 是一个 **规格驱动的工作流编排系统**，自动分析项目结构并生成定制化的治理工作流。它提供 6 个原语、8 阶段 FSM、38 个 CLI 命令，确保规格驱动的开发流程。

## 核心特性

- 🎯 **6 个原语**: Work-unit, Artifact, Contract, Check, Gate, Trace-edge
- 🔄 **8 阶段 FSM**: specify → design → plan → implement → review → test → accept → integrate
- 🛠️ **38 个 CLI 命令**: 从初始化到归档的完整生命周期管理
- 📦 **17 个 Packs**: 领域包（API、前端、后端、嵌入式、DDD）+ 意图包（feature、bugfix、refactor）
- 🔍 **22 维度 Sense**: 自动项目分析（40+ 信号）
- ✅ **质量门**: 7 个 gates + 23 个 checks，自动执行
- 🔗 **追溯性**: 双向需求跟踪 + 影响分析
- 🏗️ **Brownfield 支持**: 深度集成现有项目
- 🤖 **AI Agent 就绪**: 为 Claude Code、Codex 等 AI agent 提供 dispatch manifests
- 👥 **多 Agent 协作**: 会议协议、专家邀请、状态报告

## 仓库

| 仓库 | 说明 | 状态 |
|------|------|------|
| [core](./core) | 内核：6 原语 + FSM + 38 CLI + 17 packs | ✅ 稳定 |
| [server](./server) | HTTP/WebSocket 服务器 | 🚧 开发中 |
| [ui](./ui) | Web UI (React SPA) | 📋 计划中 |

## 快速开始

### 安装

```bash
# 全局安装（推荐）
npm install -g @spec-graph/core

# 或使用 npx（无需安装）
npx @spec-graph/core --version

# 或添加到项目
npm install --save-dev @spec-graph/core
```

### 初始化项目

```bash
# 一键初始化（推荐）
spec-graph init --quick

# 或逐步执行
spec-graph init --description "My awesome project"
spec-graph compose
spec-graph prime --bootstrap
```

### 查看状态

```bash
# 查看工作流状态
spec-graph status

# 查看富文本仪表盘
spec-graph dashboard

# 查看下一步
spec-graph next
```

### 生成 AI Agent 调度清单

```bash
# 生成 dispatch manifest
spec-graph dispatch --json

# AI agent 读取并执行
# ... agent 工作 ...

# 重新调度
spec-graph dispatch --json
```

## 使用场景

- **Web 应用** - 前端 + 后端 + API 设计 packs
- **微服务** - 多个 tracks + contract edges
- **嵌入式系统** - 固件 + 硬件 packs
- **数据管道** - 数据设计 + 迁移 packs
- **重构** - refactor pack + scope policy
- **性能优化** - performance pack + test layers

## 与其他工具对比

| 特性 | spec-graph | wdf | BMAD | spec-kit |
|------|-----------|-----|------|----------|
| 领域中立 | ✅ | ❌ (仅 Web) | ❌ | ⚠️ |
| FSM 阶段 | 8 | 4 | ⚠️ | 4 |
| 质量门 | 7 | 4 | ⚠️ | ⚠️ |
| CLI 命令 | 38 | 25 | 48 skills | 15 |
| Brownfield 支持 | ✅ (22D) | ⚠️ | ❌ | ❌ |
| 追溯性 | ✅ | ✅ | ❌ | ⚠️ |
| 多 Agent | ✅ | ❌ | ✅ | ❌ |
| Constitution | ✅ | ✅ | ❌ | ✅ |

**评分: 88/90** (16/18 维度 ⭐⭐⭐⭐⭐)

## 文档

- [快速开始指南](https://github.com/spec-graph/core#quick-start)
- [CLI 命令参考](https://github.com/spec-graph/core#commands)
- [Pack 系统](https://github.com/spec-graph/core#pack-structure)
- [AI Agent 集成](https://github.com/spec-graph/core#ai-agent-integration)

## 贡献

欢迎贡献！请阅读各仓库的 CONTRIBUTING.md 了解开发指南。

## 许可证

MIT

## 链接

- **npm**: https://www.npmjs.com/package/@spec-graph/core
- **问题反馈**: https://github.com/spec-graph/core/issues
- **讨论**: https://github.com/orgs/spec-graph/discussions

---

**spec-graph** - 让规格驱动的开发变得简单、可追溯、可治理。
