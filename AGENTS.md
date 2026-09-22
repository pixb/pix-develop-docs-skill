# AGENTS.md — pix-develop-docs-skill

Use this file as context when working with the pix-develop-docs skill.

## What This Skill Does

Analyzes a software project and generates a sequence of evolutionary development documents (00_ through 06_) that explain how to rebuild the project from scratch, phase by phase.

## When to Activate

- User asks to analyze a project
- User asks to generate development documentation
- User wants to understand project architecture
- User says "从零开始重新实现这个项目"

## Output Location

Documents are written to `docs/evolutionary-dev/` (or `{project}-dev-docs/`) at the project root.

## File Sequence

| File | Purpose |
|------|---------|
| `00_项目概述与需求分析.md` | Project background, requirements, tech stack |
| `01_核心功能拆解.md` | Module inventory, dependencies, data flow |
| `02_阶段一_*.md` | Phase 1: skeleton + core data model |
| `03_阶段二_*.md` | Phase 2: core business logic |
| `04_阶段三_*.md` | Phase 3: external integrations |
| `05_阶段四_*.md` | Phase 4: polish + edge cases |
| `06_总结与最佳实践.md` | Recap, patterns, lessons, improvements |

## Key Workflow Steps

1. Scan project structure (detect language, framework, architecture)
2. Read entry points and core modules (5-8 files)
3. Decompose into 4-6 evolutionary phases
4. Generate each document with concrete file paths and code examples
5. Verify phase ordering respects dependencies

## Quality Rules

- Code examples must use the project's actual coding style
- File paths referenced in docs must exist in the project
- Each phase must be independently testable
- No forward dependencies between phases
- Preserve original language in code snippets (don't translate comments)

## Gotchas

- Monorepo projects: ask user which package to scope to
- Circular imports in frameworks like Django: don't flag unless they cause issues
- Microservices: document per-service, not as monolith
