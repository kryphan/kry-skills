---
name: ban-ve-app
description: "Giúp người nocode vẽ 'bản vẽ' System Design cho app TRƯỚC khi build — hỏi bạn từng lớp (dữ liệu, vòng đời, phân quyền, kết nối ngoài, chịu tải, phòng hư), tự phản biện bới lỗ hổng, rồi xuất ra một bản thiết kế nền dễ đọc. Trigger khi bạn nói 'thiết kế app', 'app này cần bảng gì', 'kiến trúc cho app', 'data model', 'chốt nền kỹ thuật trước khi làm'."
version: "1.0.0"
---

# 🍜 Bản vẽ App — System Design cho người nocode

> Skill này giúp bạn **nghĩ trước cái "bản vẽ"** của app, giống chủ quán vẽ sơ đồ quán trước khi xây: bếp đặt đâu, kho ở đâu, đông khách thì luồng đi thế nào.
>
> Bạn **không cần biết code**. Bạn chỉ cần trả lời câu hỏi. AI lo phần dịch sang thuật ngữ kỹ thuật + chỉ chỗ dễ sập.
>
> **Cách dùng:** dán nguyên file này vào Claude/ChatGPT rồi mô tả app bạn muốn làm. (Hoặc để vào `.claude/skills/ban-ve-app/SKILL.md` nếu bạn xài Claude Code.)

---

## Vì sao cần

Tool nocode giỏi nhất ở khúc dựng **mặt tiền** (giao diện). Nhưng cái làm app sập khi đông khách nằm ở phần **không nhìn thấy**: dữ liệu, phân quyền, chịu tải. Đó là **System Design** — phần "nghĩ TRƯỚC".

Sửa giao diện thì rẻ. Sửa nền (đổi cách lưu dữ liệu) sau khi app đã có khách thật thì **đắt gấp 10–100 lần** — phải dọn dữ liệu cũ, có khi phải tắt app. Nên bản vẽ làm kỹ **một lần, trước khi build**.

## Nguyên tắc

1. **Mọi thứ trong bản vẽ phải sinh từ nhu cầu thật của app** — không thêm cho "sang". Cái gì không phục vụ việc gì → bỏ.
2. **Càng ít càng tốt.** Bảng thừa, quyền thừa = chỗ để hỏng.
3. **Nghĩ tới lúc tệ nhất:** đông khách, mất mạng, người dùng bấm 2 lần, kẻ xấu mò vào. Bản vẽ tốt là bản đã tính trước mấy cái đó.

---

## Quy trình — AI hỏi bạn 7 lớp

Trả lời từng lớp một. AI **không được nhảy sang code** — chỉ dựng bản vẽ. Mỗi lớp AI giải thích bằng ví dụ đời thường + kèm thuật ngữ thật (để bạn tự google sau).

**Lớp 1 — App làm gì & ai dùng?**
Một câu mô tả app. Liệt kê các loại người dùng (khách, chủ, nhân viên...). → đây là các *vai* (`roles`).

**Lớp 2 — App quản lý những "thứ" gì?**
Kể ra các danh từ chính: khách hàng, đơn hàng, sản phẩm, lịch hẹn... Mỗi "thứ" này sẽ thành **một bảng dữ liệu** (`entity` → `table`). Với mỗi thứ, liệt kê nó cần ghi những thông tin gì (`fields`).
> Ví dụ: "đơn hàng" cần ghi: ai đặt, món gì, bao nhiêu tiền, trạng thái, lúc nào.

**Lớp 3 — Mỗi thứ có vòng đời ra sao?**
Một đơn hàng đi qua các bước: *chờ thanh toán → đã trả → đang giao → xong* (hoặc *huỷ*). Vẽ ra các bước + bước nào được nhảy sang bước nào. → đây là **state machine** (máy trạng thái). Đây là chỗ nocode hay quên, dẫn tới dữ liệu loạn.

**Lớp 4 — Ai được xem/sửa gì?**
Với mỗi vai ở Lớp 1: được xem bảng nào, sửa dòng nào? Khách chỉ xem đơn của chính mình; chủ xem tất cả. → đây là **phân quyền** (`authentication` = bạn là ai · `authorization` = bạn được làm gì). Thiếu lớp này = "ai cũng đẩy cửa vô được cái két tiền".

**Lớp 5 — Nối với dịch vụ ngoài nào?**
Thanh toán, gửi email/SMS, đăng nhập Google, webhook... Với mỗi cái, hỏi: *lỡ nó lỗi thì sao? người dùng bấm 2 lần thì có bị tính tiền 2 lần không?* → đây là **boundary/contract** + **idempotency** (chống làm trùng).

**Lớp 6 — Đông khách thì sao?**
Ước lượng lúc cao điểm bao nhiêu người cùng lúc. Bàn cách "thêm bếp": **scaling** (scale up = máy mạnh hơn / scale out = thêm máy), **cache** (để sẵn thứ hay dùng cho nhanh), **CDN** (ảnh/file để gần người dùng).

**Lớp 7 — Lỡ hư thì sao?**
Có **backup** dữ liệu không? Có chỗ nào là **single point of failure** (cây cầu độc đạo — gãy là kẹt hết) không? Dữ liệu xoá nhầm có lấy lại được không (**soft-delete**)?

---

## Bước phản biện (quan trọng — đừng bỏ)

Sau khi có bản nháp, AI **tự đóng vai một kỹ sư khó tính**, cố tình bới cho ra lỗ hổng trước khi bạn build:

- Dữ liệu có thể bị loạn ở đâu? (2 người sửa cùng lúc, trạng thái nhảy lung tung)
- Kẻ xấu mò được vào dữ liệu người khác không?
- Chỗ nào bấm 2 lần thì hỏng?
- Cái gì đông khách là sập?
- Xoá nhầm có cứu được không?

AI phải nêu **ít nhất 3 điểm yếu**, bạn sửa bản vẽ cho tới khi không còn lỗ đỏ nào. (Chưa build vẫn còn rẻ để sửa.)

---

## Kết quả xuất ra

1. **Sơ đồ dữ liệu** — danh sách bảng + thông tin mỗi bảng + bảng nào nối bảng nào (dạng chữ hoặc sơ đồ ERD đơn giản).
2. **Vòng đời** các thứ quan trọng (các bước + luồng chuyển).
3. **Bảng phân quyền** — vai nào làm được gì.
4. **Danh sách kết nối ngoài** + cách xử lý khi lỗi.
5. **Ghi chú quyết định** — chọn cách làm nào, vì sao (để sau này nhớ lại).

Giữ file này lại. Khi bạn (hoặc AI) bắt tay build, cứ bám theo bản vẽ.

## Chọn công cụ (gợi ý, không bắt buộc)

Skill này lo phần **bản vẽ**, không ép công cụ. Vài lựa chọn phổ biến để bạn tự tra:
- Lưu dữ liệu (`database`): Supabase, Firebase, Airtable, hoặc DB có sẵn trong tool nocode của bạn.
- Nơi app "ở" (`hosting`): Vercel, Netlify, hoặc hosting của chính tool nocode.
- Xác thực (`auth`): tính năng auth có sẵn của nền tảng bạn dùng.

Chốt theo nhu cầu thật, đừng chọn cho "oách".

---

## KHÔNG làm

- ❌ Nhảy sang viết code/chọn màu/giao diện — đây chỉ là bản vẽ nền.
- ❌ Thêm bảng/field không phục vụ việc gì.
- ❌ Bỏ qua bước phản biện.
- ❌ Quên phân quyền, quên backup, quên chống bấm-trùng.
- ❌ Nhét mật khẩu/token vào trong bản vẽ.

---

*Skill chia sẻ từ một bài viết trong nhóm Vietnam TechMakers — "System Design cho người nocode (giải thích bằng quán ăn)". Cứ chỉnh sửa cho hợp app của bạn. Chúc build vui, đừng để quán sập giờ cao điểm 🍜*
