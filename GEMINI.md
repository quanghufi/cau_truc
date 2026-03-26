# [Tên Dự Án]

## Tổng quan

[Mục đích dự án]. [Tech stack]. [Đối tượng sử dụng].

- **Production**: [URL nếu có]
- **Status**: [Active development / Production / etc.]

## Kiến trúc

[Mô tả architecture tổng quan, các module chính]

@docs/ARCHITECTURE.md

## Tech Stack

- **Runtime**: [Node.js / Python / etc.]
- **Framework**: [FastAPI / Next.js / etc.]
- **Database**: [PostgreSQL / SQLite / etc.]
- **Testing**: [pytest / jest / etc.]

## Quy ước code

- [Naming conventions]
- [Formatting rules — ví dụ: Vietnamese cho UI text]
- [Patterns to follow — ví dụ: async/await, type hints]
- [Dependencies — ví dụ: pin exact versions]

## Hướng dẫn phát triển

```bash
# Dev server
[command]

# Tests
[command]

# Build/Deploy
[command]
```

**Platform**: Windows 11

## Agent Workflow Rules

### Core Principles
- **Simplicity First** — Mọi thay đổi đơn giản nhất có thể, impact minimal code
- **No Laziness** — Tìm root cause, không fix tạm, chuẩn senior developer
- **Minimal Impact** — Chỉ sửa thứ cần thiết, tránh tạo bug mới

### Workflow Orchestration
- Plan mode cho task ≥3 bước hoặc quyết định kiến trúc
- Nếu đi sai hướng → DỪNG và re-plan
- Dùng subagent để giữ context sạch (1 task/subagent)

### Verification Before Done
- Không mark task complete mà chưa chứng minh hoạt động
- Run tests, check logs, demonstrate correctness
- Tự hỏi: *"Staff engineer có approve không?"*

### Self-Improvement Loop
- Sau correction từ user → update `tasks/lessons.md`
- Review lessons khi bắt đầu session

### Task Management
- Plan → `tasks/todo.md`
- Lessons → `tasks/lessons.md`

## Skills
- Antigravity tự động scan `~/.gemini/antigravity/skills/`
- Không cần instruction thêm

## Lưu ý quan trọng

- [Gotchas — điều phải biết để tránh lỗi]
- [Things to avoid — anti-patterns]
