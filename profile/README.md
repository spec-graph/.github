# spec-graph

**Declaration engine — brain, not hands**

[![npm version](https://img.shields.io/npm/v/spec-graph.svg)](https://www.npmjs.com/package/spec-graph)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/node-%3E%3D18-brightgreen.svg)](https://nodejs.org)

## What is spec-graph?

spec-graph v3 is a **declaration engine**:

```
spec-graph is a brain, not hands
─────────────────────────────────
✓ Generates dispatch manifests for sub-agents
✓ Evaluates outputs through strict quality gates
✓ Tracks task lifecycle with story-level planning
✓ Enforces story gate: no story = no development
✗ Never invokes agents directly
✗ Never spawns child processes
✗ Never writes code or documents

All agent execution is delegated to external coordinators
(Claude Code skills, CI/CD, custom orchestrators)
```

It manages a 9-stage FSM, generates dispatch manifests for sub-agents, and evaluates their outputs through strict quality gates. spec-graph does not execute anything — it is the "brain" of the development process.

## Architecture

```
┌─────────────────────────────────────────────────┐
│  Skills (SKILL.md)  ← AI agent orchestration    │
│  packages/skills/    9 entry skills             │
│  init/plan/auto/status/validate/diagnose        │
│  /intervene/task/run                            │
├─────────────────────────────────────────────────┤
│  CLI (Node.js)       ← Human/agent interface     │
│  packages/cli/       25+ atomic commands        │
│  plan/dispatch/submit/task/run/sessions/...     │
├─────────────────────────────────────────────────┤
│  Core (TypeScript)   ← Declaration engine        │
│  packages/core/      13 modules                 │
│  automator/session-index/dispatch/planning      │
│  gate-enforcement/composer/recovery/...         │
└─────────────────────────────────────────────────┘
```

## Repositories

| Repo | Description | Status |
|------|-------------|--------|
| [monorepo](https://github.com/spec-graph/monorepo) | Main repo with git submodules | ✅ |
| [core](https://github.com/spec-graph/core) | Engine: automator, gate-enforcement, dispatch, session-index, planning | ✅ v3.1 |
| [cli](https://github.com/spec-graph/cli) | CLI: 25+ commands with hook automation | ✅ v3.1 |
| [skills](https://github.com/spec-graph/skills) | 9 SKILL.md files for Claude Code integration | ✅ v3.1 |
| [server](https://github.com/spec-graph/server) | HTTP/WebSocket server | 🚧 |
| [ui](https://github.com/spec-graph/ui) | Web UI (React SPA) real-time dashboard | 📋 |

## Core Capabilities

### Workflow Engine
- **9-Stage FSM**: specify → specs → design → tasks → implement → review → test → accept → integrate
- **Strict Quality Gates**: Entry/exit criteria evaluated at every transition
- **Dispatch Manifests**: Lightweight JSON with task lifecycle steps (`pre_step`/`post_step`/`complete_step`)
- **Parallel Execution**: Wave-based parallel sub-agent dispatch in implement stage
- **Progressive Recovery**: 4-level retry with Jaccard similarity detection

### Task & Story System (v3.1)
- **Task Lifecycle**: pending → running → reviewing → completed
- **Story Generation**: BMAD-style story documents with acceptance criteria, implementation plan, pseudocode, code snippets, and test plan
- **Story Gate**: `task start` requires a complete story — no story = no development
- **Session CSV Index**: Global `.spec-graph/sessions/sessions.csv` with structured IDs (`<abbrev>-<YYYYMMDD>-<NNN>`)
- **Auto-Sync**: tasks.md, state.yaml, sessions.csv automatically updated on every task transition

### Automation
- **Hook Chain**: `task start` → dispatch → sub-agent → `task review` → `task complete` → submit → loop
- **Auto-Select**: `spec-graph run` finds the latest running session automatically
- **Permission Auto-Registration**: `spec-graph init` configures full project permissions

## Quick Start

```bash
# Global install
npm install -g spec-graph

# Initialize project (registers hook + permissions)
spec-graph init

# Create session
spec-graph plan "Build JWT authentication" --confirm

# Continue workflow (auto-selects latest running session)
spec-graph run

# Generate stories for tasks (LLM fills in details)
spec-graph task story user-model

# Check all stories are complete
spec-graph task stories

# Start implementing (requires complete story)
spec-graph task start user-model

# Review and complete
spec-graph task review user-model
spec-graph task complete user-model

# Full automation via dispatch loop
spec-graph dispatch --session <id> --json
# → hook auto-chains: start → sub-agent → review → complete → submit → repeat
```

## Task Lifecycle

```
  ┌──────────┐     task start      ┌──────────┐
  │ pending  │ ──────────────────→ │ running  │
  └──────────┘                     └────┬─────┘
       ▲                               │ task review
       │                          ┌────▼─────┐
       │                          │ reviewing │
       │                          └────┬─────┘
       │                    pass  │    │ fail → fix
       │                    ┌─────┘    └──→ re-review
       │                    ▼
       │              ┌───────────┐
       └──────────────│ completed │ task complete
                      └───────────┘ (only if review passed)

  Story gate: task start requires complete story
  (no [PLACEHOLDER] markers remaining)
```

## AI Agent Integration

spec-graph provides two integration layers:

1. **Skills** (Claude Code): `/spec-graph-plan`, `/spec-graph-auto`, `/spec-graph-task`, `/spec-graph-run`, `/spec-graph-status`, `/spec-graph-intervene`
2. **CLI** (any agent): via shell commands — `dispatch --json` / `submit --result` / `task start|review|complete`

## Comparison

| Feature | spec-graph v3.1 | wdf | BMAD | spec-kit |
|---------|----------------|-----|------|----------|
| Philosophy | brain-not-hands | executor | methodology | scaffolding |
| FSM Stages | 9 | 4 | — | 4 |
| Quality Gates | 9 stage gates + task review | 4 | — | — |
| Task Tracking | pending→running→reviewing→completed | — | — | — |
| Story Planning | BMAD-style with code + pseudocode | — | ✅ | — |
| Story Gate | enforced at task start | — | — | — |
| Parallel Execution | ✅ wave-based | ❌ | ❌ | ❌ |
| Multi-Agent | ✅ meeting protocol | ❌ | ✅ | ❌ |
| Session Index | ✅ CSV with LL columns | — | — | — |
| Hook Automation | ✅ full chain | — | — | — |
| Agent-Agnostic | ✅ Claude Code + Codex + CI | — | ✅ | — |

## Documentation

- [Architecture](https://github.com/spec-graph/monorepo/blob/main/docs/ARCHITECTURE.md)
- [Agent Integration Guide](https://github.com/spec-graph/monorepo/blob/main/docs/agent-integration-guide.md)
- [v3.1 Migration Guide](https://github.com/spec-graph/monorepo/blob/main/docs/migration-3.1-session-csv.md)
- [v3.0 Migration Guide](https://github.com/spec-graph/monorepo/blob/main/docs/migration-3.0.md)

## Contributing

Contributions welcome! MIT License.

## Links

- **npm**: https://www.npmjs.com/package/spec-graph
- **Issues**: https://github.com/spec-graph/monorepo/issues
- **Discussions**: https://github.com/orgs/spec-graph/discussions
