# 💡 BRIEF: Cấu Trúc Chuẩn Cho Dự Án VS Code + AI Agents

**Ngày tạo:** 2026-03-26
**Mục tiêu:** Nghiên cứu và xây dựng cấu trúc thư mục/file chuẩn cho dự án VS Code sử dụng đồng thời 3 AI agents: **Antigravity**, **Claude Code**, và **Codex Agent**

---

## 1. VẤN ĐỀ CẦN GIẢI QUYẾT

Khi làm việc với nhiều AI coding agents cùng lúc trên 1 dự án, mỗi agent có bộ config files khác nhau. Cần có cấu trúc **chuẩn hóa**, giúp:
- Mỗi agent đọc đúng context của dự án
- Tránh xung đột giữa các agent
- Tận dụng sức mạnh riêng của từng agent
- Dễ dàng onboard dự án mới

---

## 2. NGHIÊN CỨU: TỪng Agent Cần Gì?

### 🟢 Antigravity (Google Gemini — VS Code fork)

| File/Folder | Vị trí | Chức năng |
|---|---|---|
| `GEMINI.md` | Root project | Context chính — tech stack, conventions, kiến trúc |
| `.gemini/` | Root project | Thư mục config dự án |
| `.gemini/settings.json` | `.gemini/` | Override settings cấp project |
| `.gemini/GEMINI.md` | `.gemini/` | Context bổ sung (import bằng `@file.md`) |
| `.gemini/extensions/` | `.gemini/` | MCP servers & extensions |
| `.gemini/commands/` | `.gemini/` | Custom commands (TOML) |
| `.agent/workflows/` | Root project | AWF workflow files (nếu dùng AWF) |

**Đặc biệt:**
- Hỗ trợ **hierarchical context**: Global (`~/.gemini/`) → Project (`.gemini/`) → JIT (khi mở file)
- File `GEMINI.md` có thể import file khác bằng `@docs/conventions.md`
- Extensions = MCP servers config

---

### 🟠 Claude Code (Anthropic)

| File/Folder | Vị trí | Chức năng |
|---|---|---|
| `CLAUDE.md` | Root project | Context chính — commands, code style, architecture, rules |
| `.claude/` | Root project | Thư mục config (nếu có) |
| `.claude/settings.json` | `.claude/` | Model settings, permissions |
| `tasks/todo.md` | Root project | Task tracking (best practice) |
| `tasks/lessons.md` | Root project | Lessons learned từ corrections |

**Đặc biệt:**
- `CLAUDE.md` là file TỐI QUAN TRỌNG — Claude đọc nó ĐẦU TIÊN khi mở project
- Nên gồm: Commands, Code Style, Architecture Gotchas, Agent Workflow Rules
- Hỗ trợ hierarchical: `~/.claude/CLAUDE.md` (global) → project `CLAUDE.md`

---

### 🔵 Codex Agent (OpenAI)

| File/Folder | Vị trí | Chức năng |
|---|---|---|
| `AGENTS.md` | Root project | Context chính — overview, structure, decisions, conventions |
| `.codex/` | Root project | Thư mục config dự án |
| `.codex/config.toml` | `.codex/` | Model, provider, MCP servers |
| `.agents/` | Root project | Agent run logs & prompts |
| `.feedback/` | Root project | Review feedback (nếu dùng review loop) |

**Đặc biệt:**
- `AGENTS.md` > `CODEX.md` — đây là chuẩn mới hơn, universal cho mọi AI agent
- `config.toml` chứa `model`, `model_provider`, MCP server configs
- Codex đọc `AGENTS.md` ĐẦU TIÊN

---

## 3. CẤU TRÚC CHUẨN ĐỀ XUẤT

### 3A. Cấu trúc ĐẦY ĐỦ (3 agents + project-level skills)

```
my-project/
│
├── 📄 CLAUDE.md              # Context cho Claude Code
├── 📄 AGENTS.md              # Context cho Codex (cũng là universal agent file)
├── 📄 GEMINI.md              # Context cho Antigravity/Gemini CLI
├── 📄 README.md              # Docs cho con người
├── 📄 .gitignore             # Git ignore rules
├── 📄 .env.example           # Template biến môi trường
│
├── 📁 .gemini/               # ⚙️ Antigravity config
│   ├── settings.json         #    Project-level settings
│   ├── GEMINI.md             #    Context bổ sung (modular)
│   └── extensions/           #    MCP server configs
│
├── 📁 .claude/               # ⚙️ Claude Code config
│   └── settings.json         #    Permissions, model settings
│
├── 📁 .codex/                # ⚙️ Codex config
│   └── config.toml           #    Model provider, MCP servers
│
├── 📁 .agent/                # 🤖 Shared agent resources (NẾU không dùng global)
│   ├── workflows/            #    AWF workflow files (.md)
│   ├── skills/               #    Agent skills (SKILL.md)
│   └── rules/                #    Shared rules (import từ context files)
│
├── 📁 docs/                  # 📚 Tài liệu dự án
│   ├── BRIEF.md              #    Project brief (từ /brainstorm)
│   ├── PRD.md                #    Product Requirements (từ /plan)
│   ├── ARCHITECTURE.md       #    Kiến trúc tổng quan
│   └── API.md                #    API documentation
│
├── 📁 tasks/                 # ✅ Task management
│   ├── todo.md               #    Task tracking
│   └── lessons.md            #    Lessons learned
│
├── 📁 .feedback/             # 💬 Agent review feedback
│   ├── inbox.md              #    Review findings
│   └── responses.md          #    Responses to findings
│
├── 📁 src/                   # 💻 Source code
│   └── ...
│
├── 📁 tests/                 # 🧪 Tests
│   └── ...
│
└── 📁 scripts/               # 🔧 Build/deploy scripts
    └── ...
```

### 3B. Cấu trúc TỐI GIẢN (1 agent + global skills)

Khi skills/workflows đã cài **global** (ví dụ `~/.gemini/antigravity/skills/`, `~/.claude/skills/`), project chỉ cần:

```
my-project/
├── 📄 [CONTEXT].md           # 1 file: CLAUDE.md hoặc AGENTS.md hoặc GEMINI.md
├── 📄 README.md
├── 📁 docs/
├── 📁 tasks/
│   ├── todo.md
│   └── lessons.md
├── 📁 src/
└── 📁 tests/
```

> **Lưu ý:** Chỉ cần **1 file context** cho agent đang dùng. Các folder `.gemini/`, `.claude/`, `.codex/` chỉ thêm nếu cần override settings cấp project.

### 3C. ⚠️ Skill Discovery — Agent Có Tự Tìm Skills Không?

| Agent | Tự scan global skills? | Cần làm gì? |
|---|---|---|
| **Antigravity** | ✅ **TỰ ĐỘNG** — scan `~/.gemini/antigravity/skills/` | Không cần làm gì |
| **Claude Code** | ❌ **KHÔNG** — phải được chỉ dẫn | Thêm instruction vào `CLAUDE.md` |
| **Codex Agent** | ❌ **KHÔNG** — phải được chỉ dẫn | Thêm instruction vào `AGENTS.md` |

**Giải pháp đảm bảo agent dùng skills:**

1. **Cho Claude Code** — thêm vào `CLAUDE.md` (hoặc global `~/.claude/CLAUDE.md`):
   ```
   ## Skills
   Trước khi thực hiện task, kiểm tra skills tại ~/.claude/skills/ để áp dụng guidelines phù hợp.
   ```

2. **Cho Codex Agent** — thêm vào `AGENTS.md`:
   ```
   ## Skills
   Check ~/.agent/skills/ for available skill guides before starting any task.
   ```

3. **Cho Antigravity** — *(không cần, tự động)*

> 💡 **Pro tip:** Ghi instruction vào file **global context** (`~/.claude/CLAUDE.md`) thì mọi project đều được nhắc, không cần lặp lại trong từng project.

---

## 4. NỘI DUNG CẦN CÓ TRONG MỖI FILE CONTEXT

### 4.5. Agent Workflow Rules (Chuẩn chung — tham khảo [argenisleon](https://gist.github.com/argenisleon/a477ab4d8e5595d639132e6bcac015f2))

> **Khuyến nghị:** Copy section này vào CẢ 3 file context (CLAUDE.md, AGENTS.md, GEMINI.md) để mọi agent đều tuân theo cùng 1 bộ quy tắc.

#### Workflow Orchestration
- **Plan-First**: Enter plan mode cho mọi task ≥3 bước hoặc có quyết định kiến trúc
- **Stop & Re-plan**: Nếu đi sai hướng → DỪNG và lên plan lại, đừng cố push tiếp
- **Subagent Strategy**: Dùng subagent để giữ context window chính sạch (1 task/subagent)

#### Core Principles
- **Simplicity First**: Mọi thay đổi đơn giản nhất có thể, impact minimal code
- **No Laziness**: Tìm root cause, không fix tạm, giữ chuẩn senior developer
- **Minimal Impact**: Chỉ sửa thứ cần thiết, tránh tạo bug mới

#### Verification Before Done
- **Không bao giờ** mark task complete mà chưa chứng minh nó hoạt động
- Run tests, check logs, demonstrate correctness
- Tự hỏi: *"Staff engineer có approve cái này không?"*

#### Self-Improvement Loop
- Sau MỌI correction từ user → update `tasks/lessons.md` với pattern
- Viết rules ngăn lặp lại sai lầm
- Review lessons khi bắt đầu session

#### Autonomous Bug Fixing
- Khi nhận bug report → just fix it, không hỏi hand-holding
- Point at logs, errors, failing tests → rồi resolve
- Zero context switching từ user

#### Demand Elegance (Balanced)
- Non-trivial changes: pause hỏi *"Có cách nào elegant hơn không?"*
- Fix hacky: *"Knowing everything I know now, implement the elegant solution"*
- Skip cho simple, obvious fixes — đừng over-engineer

---

### CLAUDE.md — Template

```markdown
# [Tên dự án]

[Mô tả ngắn 1-2 dòng]. [Tech stack].

- **Production**: [URL]

## Commands

\```bash
# Dev server
[command]

# Tests
[command]

# Deploy
[command]
\```

**Platform**: Windows 11 — dùng `;` thay `&&` trong shell.

## Code Style

- [Ngôn ngữ UI]
- [Coding conventions]
- [Dependencies management]

## Architecture Gotchas

### [Gotcha 1]
[Mô tả chi tiết]

### [Gotcha 2]
[Mô tả chi tiết]

## Testing

- [Framework], coverage threshold [X]%
- IMPORTANT: chạy tests trước khi commit

## Security

- [Security rules quan trọng]
- IMPORTANT: security review trước khi deploy

## Skills

- Trước khi thực hiện task, kiểm tra skills tại `~/.claude/skills/`
- Áp dụng guidelines phù hợp từ SKILL.md files

## Agent Workflow Rules

### Core Principles
- **Simplicity First** — Make every change as simple as possible
- **No Laziness** — Find root causes; no temporary fixes
- **Minimal Impact** — Touch only what's necessary

### Workflow Orchestration
- Enter plan mode for any non-trivial task (3+ steps)
- If things go sideways, stop and re-plan
- Use subagents liberally; one task per subagent

### Verification Before Done
- Never mark a task complete without proving it works
- Run build/test commands, check logs
- Ask: "Would a staff engineer approve this?"

### Self-Improvement Loop
- After any user correction: update `tasks/lessons.md`
- Review lessons at session start

### Task Management
1. Write plan to `tasks/todo.md` with checkable items
2. Check in with user before implementation
3. Mark items complete as you go
4. Capture lessons in `tasks/lessons.md` after corrections
```

---

### AGENTS.md — Template

```markdown
# [Tên dự án]

## Project Overview

[Mô tả tổng quan]. [Status].

## Tech Stack

- **Runtime:** [runtime]
- **Language:** [language]
- **Frameworks:** [frameworks]

## Project Structure

\```
project/
├── src/
├── tests/
├── docs/
├── tasks/
│   ├── todo.md
│   └── lessons.md
└── ...
\```

## Key Decisions

- [Decision 1] — [Rationale]
- [Decision 2] — [Rationale]

## Conventions

- [Commit style]
- [Code style]
- [File organization rules]

## Agent Workflow Rules

### Core Principles
- **Simplicity First** — Minimal code impact
- **No Laziness** — Root causes, senior standards
- **Minimal Impact** — Only touch what's necessary

### Workflow
- Plan mode for 3+ steps tasks
- Subagent strategy: 1 task per subagent
- Verify before marking done

### Task Management
- Plan → `tasks/todo.md`
- Lessons → `tasks/lessons.md`

## Skills

- Check `~/.agent/skills/` for available skill guides before tasks
- Apply relevant SKILL.md guidelines

## Important: Do NOT

- [Anti-pattern 1]
- [Anti-pattern 2]
- [Gotcha 1]
```

---

### GEMINI.md — Template

```markdown
# [Tên dự án]

## Tổng quan
[Mục đích]. [Tech stack]. [Đối tượng sử dụng].

## Kiến trúc
[Architecture tổng quan, module chính]

@docs/ARCHITECTURE.md

## Quy ước code
- [Naming conventions]
- [Formatting rules]
- [Patterns to follow]

## Hướng dẫn phát triển
\```bash
# Dev
[command]

# Test
[command]

# Build
[command]
\```

## Agent Workflow Rules

### Core Principles
- **Simplicity First** — Mọi thay đổi đơn giản nhất có thể
- **No Laziness** — Tìm root cause, không fix tạm
- **Minimal Impact** — Chỉ sửa cần thiết

### Workflow
- Plan mode cho task ≥3 bước
- Dùng subagent để giữ context sạch
- Verify trước khi mark done

### Task Management
- Plan → `tasks/todo.md`
- Lessons → `tasks/lessons.md`

## Skills
- Antigravity tự động scan `~/.gemini/antigravity/skills/`
- Không cần instruction thêm

## Lưu ý quan trọng
- [Gotchas]
- [Things to avoid]
```

---

## 5. BEST PRACTICES — LÀM SAO CHO 3 AGENT KHÔNG XÂY ĐỘT?

### 5.1. Nguyên tắc DRY (Don't Repeat Yourself)

Tạo 1 file `docs/ARCHITECTURE.md` chứa chi tiết kiến trúc, rồi **reference** từ cả 3 file context:
- `CLAUDE.md`: *"Architecture details → see `docs/ARCHITECTURE.md`"*
- `AGENTS.md`: *"Architecture → `docs/ARCHITECTURE.md`"*
- `GEMINI.md`: `@docs/ARCHITECTURE.md` (native import)

### 5.2. Phân vai cho từng Agent

| Vai trò | Agent tốt nhất | Lý do |
|---|---|---|
| **Coding chính** | Antigravity | Context window lớn, subagents, browser tool |
| **Code review** | Codex Agent | Sandbox isolation, structured review output |
| **Debugging** | Claude Code | Extended thinking, root cause analysis |
| **Architecture** | Claude Code | Deep reasoning + large context |
| **Quick fixes** | Codex Agent | Fast, autonomous, minimal overhead |
| **UI/Frontend** | Antigravity | Browser subagent, screenshot tool |

### 5.3. File nào `.gitignore`?

```gitignore
# AI Agent local configs (KHÔNG commit)
.claude/
.codex/
.agents/

# AI Agent context files (NÊN commit)
# CLAUDE.md, AGENTS.md, GEMINI.md → commit
# .gemini/ → commit (trừ cache)
# .agent/workflows/ → commit
# tasks/ → commit
# .feedback/ → tùy team
```

### 5.4. Sync nội dung giữa 3 files

Khi update 1 file context, **nhớ sync các file khác** nếu thay đổi ảnh hưởng chung:
- Thêm command mới → Update cả 3
- Thay đổi architecture → Update `docs/ARCHITECTURE.md` (referenced)
- Thêm convention → Update cả 3
- Thêm gotcha → Update cả 3

---

## 6. ƯỚC TÍNH SƠ BỘ

| Hạng mục | Giá trị |
|---|---|
| **Số files config tối thiểu** | 6 files (3 context + 3 config dirs) |
| **Số files config đầy đủ** | ~15-20 files |
| **Độ phức tạp setup** | Trung bình — cần hiểu từng agent |
| **Maintainability** | Cần discipline để sync 3 files context |
| **Rủi ro** | Các agent có thể sửa đè file của nhau nếu không cẩn thận |

---

## 7. BƯỚC TIẾP THEO

| # | Hành động | Command |
|---|---|---|
| 1️⃣ | Xem lại Brief, sửa nếu cần | — |
| 2️⃣ | Tạo template project với cấu trúc đề xuất | `/plan` |
| 3️⃣ | Tạo script tự động generate 3 files context | `/code` |
| 4️⃣ | Lưu kiến thức này vào Neural Memory | `/save-brain` |
