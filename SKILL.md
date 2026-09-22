---
name: pix-develop-docs-skill
description: Analyze a project from a "re-implementation" perspective and generate evolutionary development documentation. Activates with phrases like analyze project, generate development docs, understand project, create project documentation, project analysis, explain project architecture. Produces numbered markdown document sequences (00_ through 06_) that walk through rebuilding the project from scratch using incremental phases.
activation: /pix-develop-docs
license: MIT
author: pix
version: 1.0.0
metadata:
  author: pix
  version: 1.0.0
  created: 2026-09-22
  last_reviewed: 2026-09-22
  review_interval_days: 90
  dependencies: []
  schema_expectations: {}
provenance:
  maintainer: pix
  version: 1.0.0
  created: 2026-09-22
  source_references: []
---

# /pix-develop-docs

Generate evolutionary development documentation that explains how to rebuild a project from scratch, phase by phase.

## Trigger Examples

- "analyze this project and generate development docs"
- "帮我理解这个项目的架构"
- "generate the evolutionary dev docs for this codebase"
- "create project documentation from re-implementation perspective"
- "从零开始重新实现这个项目"

## When to Use

- User wants to understand a complex unfamiliar project
- Onboarding to a new codebase
- Creating architecture documentation
- Preparing technical handoff or knowledge transfer
- User asks "how would I build this from scratch?"

## When NOT to Use

- User wants API documentation (use api-doc skill)
- User wants a README (use readme-gen skill)
- User wants code comments only
- Project is trivial (< 5 source files, single module)

## Workflow

### Step 1: Project Scan

Scan the project root to determine:

1. **Language & Framework** — look for lock files and config:
   - `package.json` / `pnpm-lock.yaml` → Node.js
   - `go.mod` → Go
   - `requirements.txt` / `pyproject.toml` / `Pipfile` → Python
   - `Cargo.toml` → Rust
   - `pom.xml` / `build.gradle` → Java
   - `Gemfile` → Ruby
   - `composer.json` → PHP

2. **Architecture pattern** — check top-level directories:
   - `cmd/`, `internal/`, `pkg/` → Go standard layout
   - `src/`, `dist/`, `public/` → Frontend app
   - `server/`, `handlers/`, `models/` → Backend service
   - `lib/`, `tests/` → Library/package
   - `infra/`, `terraform/`, `k8s/` → Infrastructure project

3. **Entry points** — find `main.*`, `index.*`, `app.*`, `__main__.py`, `main.go`

4. **Dependencies** — parse the primary manifest file for the full dependency tree

Read the entry point files and 3-5 most important module files to understand the core business logic.

### Step 2: Functional Decomposition

Break the project into 4-6 evolutionary phases. Each phase must:

- Be independently deployable/testable
- Build on the previous phase
- Cover a coherent set of functionality
- Follow this ordering:

```
Phase 1: Project skeleton + core data model
Phase 2: Core business logic (the "must-have" feature)
Phase 3: External integrations (APIs, DB, file I/O)
Phase 4: Secondary features (auth, config, logging)
Phase 5: Polish (error handling, edge cases, performance)
```

For each phase, identify:
- **Input**: what data/config goes in
- **Output**: what it produces
- **Dependencies**: what prior phases it needs
- **Verification**: how to confirm it works (test command, curl, manual check)

### Step 3: Document Generation

Create a directory at the project root named `docs/evolutionary-dev/` (or `{project}-dev-docs/`). Generate the following files sequentially:

#### File: `00_项目概述与需求分析.md`

Contents:
- Project background (1 paragraph: what problem it solves, for whom)
- Core functional requirements (numbered list, 5-10 items)
- Non-functional requirements (performance, security, scalability)
- Tech stack table: language, framework, key libraries with versions
- Directory tree with 1-line purpose per entry
- Architecture overview (1 paragraph + diagram reference if applicable)
- Dev environment setup (exact commands to clone, install, run)

#### File: `01_核心功能拆解.md`

Contents:
- Module inventory: table with module name, purpose, file count, complexity
- Dependency graph: which modules import/call which
- Data flow: entry → processing → storage → response
- Priority matrix: must-have vs nice-to-have features
- API surface: public endpoints/functions with signatures

#### Files: `02_阶段一_*.md` through `05_阶段四_*.md`

Each phase file must contain:

```markdown
# Phase N: [Phase Name]

## Objective
[One sentence: what this phase delivers]

## Requirements
[What needs to work after this phase]

## Technical Design
[Architecture decisions, patterns chosen, and WHY]

## Implementation Steps
1. [Concrete step with file paths]
   - Create `src/models/user.py` with User dataclass
   - Fields: id (UUID), email (str), created_at (datetime)
   - Validation: email must match RFC 5322
2. [Next step]
   ...

## Key Decisions
| Decision | Alternative Rejected | Reason |
|----------|---------------------|--------|
| Use UUID v7 | Auto-increment ID | Better for distributed systems |

## Verification
- Run: `python -m pytest tests/test_phase_n.py`
- Expected: all green, coverage > 80%
- Manual: [specific manual check]
```

#### File: `06_总结与最佳实践.md`

Contents:
- Complete architecture recap
- Design patterns used (with file:line references)
- Performance characteristics
- Security considerations
- Lessons learned from the analysis
- Suggested improvements

### Step 4: Quality Checks

After generating all documents:

1. Verify each phase builds logically on the previous
2. Check that file paths referenced in docs actually exist in the project
3. Ensure code snippets are syntactically valid
4. Confirm the evolution order is dependency-respectful (no forward refs)
5. Validate total doc directory size is reasonable (< 50KB)

## Analysis Methodology

### Code Reading Order

1. Entry point → understand boot sequence
2. Config → understand environment and secrets
3. Models/types → understand data shapes
4. Core business logic → understand the "what"
5. Integrations → understand the "how"
6. Tests → understand expected behavior
7. Error handling → understand failure modes

### Decision Inference Rules

When the codebase doesn't explain *why* a decision was made:

- **Pattern observed** → look for the alternative that was rejected
- **Library chosen** → check if a lighter/simpler alternative exists and was passed over
- **Architecture style** → identify if it solves a specific scaling/reliability problem
- **Naming convention** → check if it aligns with the framework's ecosystem norms

Record each inferred decision with evidence. Mark low-confidence inferences explicitly.

### Anti-Pattern Detection

Flag these during analysis and note them in the final docs:

- God classes/modules (> 500 lines doing multiple things)
- Circular dependencies
- Configuration scattered across files
- Missing error handling on I/O operations
- Hardcoded values that should be configurable
- Test coverage gaps in core logic

## Output Format

All documents use this naming convention: `NN_中文标题.md`

- NN: two-digit zero-padded number starting from 00
- Title: Chinese, descriptive, no special characters
- Encoding: UTF-8
- Line endings: LF
- Max file size: 15KB per document

## Gotchas

- Projects with monorepo structure (e.g., pnpm workspaces, Go modules) need the analysis scoped to a single package — ask the user which package to focus on
- Generated code examples should use the project's actual coding style, not a generic style guide — detect this from existing source files
- Some projects have circular imports by design (e.g., Django apps) — do not flag these as anti-patterns without evidence they cause problems
- Non-English projects may use mixed-language comments — preserve the original language in code snippets
- Microservice architectures should be documented per-service, not as a monolith — detect by checking for independent deploy units (Dockerfiles, docker-compose services)

## Cross-Platform Compatibility

This skill works on any platform that supports markdown document generation:

| Platform | Notes |
|----------|-------|
| Claude Code | Native support, full workflow |
| Cursor | Native support, full workflow |
| Codex CLI | Native support, full workflow |
| Copilot | Works, may need manual file creation |
| Trae | Native support, full workflow |
| Windsurf | Native support, full workflow |
| Zed | Works with assistant mode |
| Continue.dev | Works with chat mode |
