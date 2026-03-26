# 🧠 Anti-Overthinking Rules for AI Agents

> Mục đích: Ngăn AI agents overthink — tránh over-plan, over-explain, over-engineer.
> Áp dụng cho: Antigravity, Claude Code, Codex Agent (và mọi AI coding agent khác).

---

## Vấn đề: Agent hay overthink như thế nào?

| Triệu chứng | Ví dụ |
|---|---|
| **Over-plan** | Tạo checklist 20 items cho task sửa 1 dòng code |
| **Over-explain** | Viết 500 chữ giải thích trước khi sửa 1 bug hiển nhiên |
| **Over-engineer** | Tạo abstraction layer cho 1 hàm đơn giản |
| **Over-verify** | Chạy 10 bước kiểm tra cho thay đổi CSS |
| **Over-ask** | Hỏi 5 câu xác nhận khi user đã nói rõ yêu cầu |
| **Over-refactor** | Sửa code không liên quan đến task được giao |

---

## ⚠️ Safety Exception — Luôn cần plan/clarification

> **Quan trọng:** Các rules dưới đây **KHÔNG ÁP DỤNG** khi task thuộc loại:
> - 🔴 **Destructive actions** — xóa data, xóa file, drop table
> - 🔴 **Production/config changes** — deploy, env vars, infrastructure
> - 🔴 **Auth/security changes** — authentication, authorization, secrets
> - 🔴 **Schema/data migrations** — database schema, data transformations
> - 🔴 **Unclear acceptance criteria** — user chưa nói rõ muốn gì
>
> Với các trường hợp trên: **BẮT BUỘC** plan mode + xác nhận với user trước khi thực hiện.

---

## Rules — Copy vào file context (CLAUDE.md / AGENTS.md / GEMINI.md)

### Rule 1: Task Complexity → Response Size

```
Task đơn giản, rõ ràng, rủi ro thấp  → Sửa trực tiếp, KHÔNG plan mode
Task ít code, tự giải thích           → KHÔNG giải thích, chỉ sửa
Task user đã nói rõ yêu cầu           → KHÔNG hỏi xác nhận, làm luôn
```

> **Heuristics** (tín hiệu, không phải hard rule):
> - Số files ít (1-2) là tín hiệu đơn giản, nhưng 1 file auth vẫn có thể nguy hiểm
> - Số dòng ít (< 10) thường đơn giản, nhưng xét thêm blast radius + reversibility
> - Khi nghi ngờ → ưu tiên an toàn (plan mode)

### Rule 2: Quick Fix Protocol

Cho task đơn giản (fix typo, thêm 1 field, sửa 1 bug rõ ràng):
1. **KHÔNG** plan mode
2. **KHÔNG** hỏi xác nhận
3. **KHÔNG** giải thích dài
4. Sửa → verify → done

### Rule 3: Giới hạn Scope

- Không refactor code không liên quan đến task hiện tại
- Không đề xuất "improvements" khi không được hỏi
- Không tạo abstraction cho thứ chỉ dùng 1 lần
- Không thêm error handling cho case không bao giờ xảy ra

### Rule 4: Explanation Budget

```
Nếu explanation dài hơn code change → viết ngắn lại
Nếu code tự giải thích → không cần explanation
Nếu user nói "làm đi" → làm luôn, 0 giải thích
```

### Rule 5: Plan Mode Threshold

| Điều kiện | Hành động |
|---|---|
| 1-2 files, logic rõ ràng, rủi ro thấp | ➡️ Sửa trực tiếp |
| 3+ files hoặc logic phức tạp | ➡️ Plan mode ngắn gọn |
| Thay đổi kiến trúc | ➡️ Plan mode + hỏi user |
| Bug rõ nguyên nhân | ➡️ Fix luôn |
| Bug chưa rõ nguyên nhân | ➡️ Debug → Fix → Giải thích ngắn |

### Rule 6: "Khi user nói X, làm Y"

| User nói | Agent phải làm |
|---|---|
| "sửa lỗi này" | Fix luôn, không hỏi lại |
| "làm đi" / "tiến hành" | Execute, không plan thêm |
| "thêm [feature]" | Implement, hỏi chỉ khi ambiguous |
| "tại sao lỗi?" | Giải thích ngắn + fix |
| "@file sửa dòng X" | Bắt đầu từ dòng đó, mở rộng đến dòng lân cận nếu cần cho fix đúng root cause. Hỏi user chỉ khi dòng đó đã thay đổi hoặc fix vượt scope ban đầu |

---

## Copy-Paste Block — Thêm vào Context File

### Cho CLAUDE.md:

```markdown
## Anti-Overthinking
- Task đơn giản, rõ ràng → sửa trực tiếp, không plan mode
- Nếu explanation dài hơn code change → viết ngắn lại  
- Không refactor code không liên quan đến task hiện tại
- Không đề xuất "improvements" khi không được hỏi
- Khi user nói "làm đi" → execute ngay, không hỏi lại
- Bug rõ ràng → fix luôn, không giải thích dài dòng
- ⚠️ Task phức tạp, high-risk, hoặc kiến trúc → vẫn cần plan/clarification
- ⚠️ Luôn verify trước khi mark done
```

### Cho AGENTS.md:

```markdown
## Anti-Overthinking
- Simple, low-risk tasks → fix directly, no plan mode
- If explanation is longer than code change → shorten it
- Don't refactor unrelated code
- Don't suggest "improvements" unless asked
- When user says "do it" → execute immediately
- Obvious bugs → just fix, minimal explanation
- ⚠️ Complex, high-risk, or architectural changes → still require plan/clarification
- ⚠️ Always verify before marking done
```

### Cho GEMINI.md:

```markdown
## Anti-Overthinking
- Task đơn giản, rõ ràng → sửa trực tiếp, không mở rộng scope ngoài task
- Explanation ngắn hơn code change
- Không refactor ngoài scope
- User nói "làm đi" → execute, không hỏi
- Bug rõ → fix luôn
- ⚠️ Task phức tạp, high-risk → vẫn cần plan mode + verify trước khi done
```

---

## Mức độ Overthinking theo Agent (khuynh hướng phổ biến)

> **Lưu ý:** Bảng dưới phản ánh khuynh hướng *thường thấy trong setup này*,
> không phải đặc tính cố định. Hành vi thực tế thay đổi theo system prompt, tools, và version.

| Agent | Khuynh hướng | Nguyên nhân thường gặp | Rule nên ưu tiên |
|---|---|---|---|
| **Claude Code** | 🔴 Thường over-explain | Hay giải thích dài, hay hỏi xác nhận | Rule 4 (Explanation Budget) |
| **Antigravity** | 🟡 Thường over-plan | Hay tạo task.md, artifact cho mọi thứ | Rule 5 (Plan Threshold) |
| **Codex** | 🟢 Ít overthink hơn | Autonomous hơn, direct hơn | Rule 3 (Scope) |

---

## Lưu ý

> **Rules này BỔ SUNG cho Agent Workflow Rules, không thay thế.**
> Agent vẫn cần plan mode cho task phức tạp, vẫn cần verify trước khi done.
> Anti-Overthinking chỉ ngăn agent **làm quá mức cần thiết** cho task đơn giản.
