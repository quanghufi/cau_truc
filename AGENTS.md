# [Tên Dự Án]

## Project Overview

[Mô tả tổng quan dự án]. [Status: Active development / Production / etc.]

## Tech Stack

- **Runtime:** [Node.js 20+ / Python 3.12 / etc.]
- **Language:** [JavaScript / TypeScript / Python / etc.]
- **Frameworks:** [FastAPI / Next.js / etc.]
- **Database:** [PostgreSQL / SQLite / etc.]

## Project Structure

```
project/
├── src/                    # Source code
│   └── ...
├── tests/                  # Test files
│   └── ...
├── docs/                   # Documentation
│   ├── BRIEF.md            # Project brief
│   ├── ARCHITECTURE.md     # Architecture overview
│   └── API.md              # API docs
├── tasks/                  # Task management
│   ├── todo.md             # Current tasks
│   └── lessons.md          # Lessons learned
├── scripts/                # Build/deploy scripts
├── CLAUDE.md               # Claude Code context
├── AGENTS.md               # This file (Codex context)
├── GEMINI.md               # Antigravity context
└── README.md               # Human documentation
```

## Key Decisions

- [Decision 1] — [Rationale]
- [Decision 2] — [Rationale]

## Conventions

- Commit messages follow conventional commits
- [Code style rules]
- [File organization rules]

### File Organization
- Keep modules reasonably organized and easy to review
- Split files when it clearly improves maintainability
- Prefer coherent domain boundaries over mechanical file splitting

## Skills

- Check `~/.agent/skills/` for available skill guides before tasks
- Apply relevant SKILL.md guidelines

## Agent Workflow Rules

### Core Principles
- **Simplicity First** — Make every change as simple as possible; minimal code impact
- **No Laziness** — Find root causes; no temporary fixes; senior developer standards
- **Minimal Impact** — Changes should only touch what's necessary

### Workflow Orchestration
- Enter plan mode for any non-trivial task (3+ steps)
- If things go sideways, stop and re-plan immediately
- Use subagents for focused execution; one task per subagent

### Verification Before Done
- Never mark a task complete without proving it works
- Run tests, check logs, demonstrate correctness
- Ask: "Would a staff engineer approve this?"

### Self-Improvement Loop
- After ANY correction from the user: update `tasks/lessons.md`
- Review lessons at session start for relevant project

### Task Management
- Plan → `tasks/todo.md` with checkable items
- Lessons → `tasks/lessons.md` after corrections

## Important: Do NOT

- [Anti-pattern 1 — ví dụ: Modify evidence files]
- [Anti-pattern 2 — ví dụ: Use exec() for production]
- [Gotcha 1 — ví dụ: Count TIMEOUT as passing test]

## Anti-Overthinking
- Simple, low-risk tasks → fix directly, no plan mode
- If explanation is longer than code change → shorten it
- Don't refactor unrelated code
- Don't suggest "improvements" unless asked
- When user says "do it" → execute immediately
- Obvious bugs → just fix, minimal explanation
- ⚠️ Complex, high-risk, or architectural changes → still require plan/clarification
- ⚠️ Always verify before marking done
