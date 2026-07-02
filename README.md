# 🧩 kry-skills

Một bộ **AI skill template** — mấy file `.md` bạn dán vào Claude/ChatGPT (hoặc bỏ vào `.claude/skills/`) để AI làm việc theo một quy trình có sẵn, thay vì phải prompt lại từ đầu mỗi lần.

Viết cho **người nocode / non-tech** là chính: giải thích bằng ví dụ đời thường + kèm thuật ngữ thật để bạn tự đào sâu.

> Trích dần từ bộ skill mình dùng thật, chia sẻ qua nhóm **Vietnam TechMakers**. Repo sẽ có thêm skill theo thời gian. Cứ tải về, chỉnh cho hợp việc của bạn. Thấy hữu ích thì cho mình xin 1 star ⭐ để có động lực làm tiếp.

---

## Skill trong bộ

| Skill | Dùng để làm gì |
|---|---|
| [`kry-ux`](kry-ux/SKILL.md) | Đóng đinh **hành vi** app **trước khi build** — có màn nào, nút nào làm gì, luồng ra sao, lúc rỗng/lỗi/mất mạng hiện gì. Chạy theo khung đếm được nên không sót màn, không sót case. |
| [`kry-system`](kry-system/SKILL.md) | Vẽ "bản vẽ" **System Design** cho app **trước khi build** — hỏi bạn 7 lớp (dữ liệu, vòng đời, phân quyền, kết nối ngoài, chịu tải, phòng hư) rồi tự phản biện bới lỗ hổng. Giải thích bằng ẩn dụ "mở quán ăn". |

> **Thứ tự dùng khi làm app mới:** `kry-ux` (chốt hành vi) → `kry-system` (chốt nền) → rồi mới build.

*(còn nữa — sẽ bổ sung dần)*

## Cách dùng (3 cách)

**1. Nhanh nhất — dán vào chat:**
Mở skill bạn cần (vd [`kry-system/SKILL.md`](kry-system/SKILL.md)), copy toàn bộ nội dung, dán vào Claude hoặc ChatGPT, rồi mô tả việc bạn muốn làm.

**2. Dùng với Claude Code:**
Tải folder skill (vd `kry-system/`) bỏ vào `~/.claude/skills/` (hoặc `.claude/skills/` trong project). Gõ `/kry-system` hoặc nói "thiết kế app giúp mình".

**3. Clone cả bộ:**
```bash
git clone https://github.com/kryphan/kry-skills.git
```

---

*Made by [KryPhan](https://github.com/kryphan) · vừa làm vừa học, thấy sai chỗ nào cứ mở issue 🙂*
