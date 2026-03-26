# [Tên Dự Án]

[Mô tả ngắn 1-2 dòng]. [Tech stack].

- **Production**: [URL nếu có]

## Commands

```bash
# Dev server
[command]

# Tests
[command]

# Deploy
[command]
```

**Platform**: Windows 11 — dùng `;` thay `&&` trong shell.

## Code Style

- Vietnamese cho mọi text hiển thị cho user
- [Coding conventions — ví dụ: async/await, type hints]
- [Dependencies management — ví dụ: pin exact versions]

## Architecture Gotchas

### [Tên Gotcha 1]
[Mô tả chi tiết — điều agent PHẢI biết để không mắc lỗi]

### [Tên Gotcha 2]
[Mô tả chi tiết]

## Testing

- [Framework — ví dụ: pytest, jest], coverage threshold [X]%
- IMPORTANT: chạy tests trước khi commit any code changes

## Security

- [Security rules quan trọng]
- IMPORTANT: luôn chạy security review trước khi deploy production

## Skills

- Trước khi thực hiện task, kiểm tra skills tại `~/.claude/skills/`
- Áp dụng guidelines phù hợp từ SKILL.md files

## Agent Workflow Rules

### Core Principles
- **Simplicity First** — Make every change as simple as possible; minimize code impact
- **No Laziness** — Find root causes; no temporary fixes; hold to senior developer standards
- **Minimal Impact** — Touch only what's necessary; avoid introducing new bugs

### Workflow Orchestration
- Enter plan mode for any non-trivial task (3+ steps or architectural decisions)
- If things go sideways, stop and re-plan — don't keep pushing
- Use subagents liberally to keep the main context window clean; one task per subagent
- For non-trivial changes, pause and ask if a more elegant approach exists

### Verification Before Done
- Never mark a task complete without proving it works
- Run build/test commands to verify compilation, run tests, check logs
- Ask: *"Would a staff engineer approve this?"*

### Autonomous Bug Fixing
- When given a bug report: just fix it — no hand-holding
- Point at logs, errors, failing tests — then resolve them
- Zero context switching required from the user

### Self-Improvement Loop
- After any user correction: update `tasks/lessons.md` with the pattern
- Write rules to prevent repeating the same mistake
- Review lessons at session start

### Task Management
1. Write plan to `tasks/todo.md` with checkable items before implementation
2. Check in with user before implementation begins
3. Mark items complete as you go
4. Provide high-level summary at each step
5. Capture lessons in `tasks/lessons.md` after corrections

## Anti-Overthinking
- Task đơn giản, rõ ràng → sửa trực tiếp, không plan mode
- Nếu explanation dài hơn code change → viết ngắn lại  
- Không refactor code không liên quan đến task hiện tại
- Không đề xuất "improvements" khi không được hỏi
- Khi user nói "làm đi" → execute ngay, không hỏi lại
- Bug rõ ràng → fix luôn, không giải thích dài dòng
- ⚠️ Task phức tạp, high-risk, hoặc kiến trúc → vẫn cần plan/clarification
- ⚠️ Luôn verify trước khi mark done
