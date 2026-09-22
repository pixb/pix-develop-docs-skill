# pix-develop-docs-skill

Analyze a project from a "re-implementation" perspective and generate evolutionary development documentation.

## What It Does

Walks through a codebase and produces a numbered document sequence (00_ through 06_) that explains how to rebuild the project from scratch, phase by phase. Each phase is independently verifiable.

## Installation

### Claude Code

```bash
# Copy to your skills directory
cp -r pix-develop-docs-skill ~/.claude/skills/
```

### Cursor

```bash
# Copy to your skills directory
cp -r pix-develop-docs-skill ~/.cursor/skills/
```

### Codex CLI

```bash
# Copy to your skills directory
cp -r pix-develop-docs-skill ~/.codex/skills/
```

### Trae

```bash
# Copy to your skills directory
cp -r pix-develop-docs-skill ~/.trae/skills/
```

### Generic (any platform)

Clone or copy the directory to wherever your agent looks for skills, then restart the agent session.

## Usage

In your agent session, say:

```
analyze this project and generate development docs
```

or:

```
帮我理解这个项目的架构
```

or:

```
从零开始重新实现这个项目
```

## Output

A `docs/evolutionary-dev/` directory with:

| File | Content |
|------|---------|
| `00_项目概述与需求分析.md` | Background, requirements, tech stack |
| `01_核心功能拆解.md` | Module inventory, dependencies, data flow |
| `02_阶段一_*.md` | Phase 1: skeleton + core data model |
| `03_阶段二_*.md` | Phase 2: core business logic |
| `04_阶段三_*.md` | Phase 3: external integrations |
| `05_阶段四_*.md` | Phase 4: polish + edge cases |
| `06_总结与最佳实践.md` | Recap, patterns, lessons, improvements |

## Supported Projects

Any programming language or framework — the skill auto-detects:

- Node.js / TypeScript / JavaScript
- Python
- Go
- Rust
- Java / Kotlin
- Ruby
- PHP
- And more

## Templates

The `templates/` directory contains fill-in templates for each document. These are references for the agent, not files to be edited manually.

## License

MIT
