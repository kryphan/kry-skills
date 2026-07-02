---
name: kry-ux
description: "Giúp bạn đóng đinh HÀNH VI của app TRƯỚC khi build — có những màn nào, nút nào làm gì, luồng đi ra sao, lúc rỗng/lỗi/mất mạng thì hiển thị gì. Chạy theo khung đếm được nên không sót màn, không sót case. Trigger khi bạn nói 'app này có những màn gì', 'thiết kế luồng', 'spec UX', 'flow đăng ký/mua hàng thế nào', 'đóng đinh hành vi trước khi code'."
version: "1.0.0"
---

# kry-ux — Đặc tả hành vi app trước khi build 📝

> Trước khi bảo AI (hoặc thuê ai đó) build, bạn phải nói rõ app **làm gì, phản ứng ra sao** trong mọi tình huống. Nếu không, AI sẽ tự đoán — và đoán sai.
>
> Skill này biến "ý tưởng trong đầu" thành **bản đặc tả hành vi (UX spec)** đầy đủ, đếm được, để lúc build không cãi nhau "ơ tưởng nó phải thế này".
>
> Đi đôi với [`kry-system`](../kry-system/SKILL.md): **kry-ux** chốt *hành vi* (màn, nút, luồng), **kry-system** chốt *nền* (dữ liệu, phân quyền). Làm kry-ux trước.
>
> **Cách dùng:** dán file này vào Claude/ChatGPT rồi mô tả app bạn muốn làm. (Hoặc bỏ folder `kry-ux/` vào `.claude/skills/`.)

---

## 2 luật cứng (đọc trước)

**1. Lấy INTENT của người dùng làm gốc — không lấy hệ thống làm gốc.**
Ở mỗi bước, hỏi: *ngay khoảnh khắc này, người dùng đang muốn gì, đang lo/nghi điều gì?* → đáp ứng nhanh nhất, ít bắt họ suy nghĩ nhất. Chọn kiểu hiển thị (popup? trang mới? thông báo nhỏ?) là để **phục vụ ý định đó**, không phải cho đẹp.

**2. Độ phủ đến từ ĐẾM, không từ "nghĩ ra được bao nhiêu".**
Đừng brainstorm tới lúc thấy "chắc đủ rồi". Luôn liệt kê theo một **khung đếm được** (bên dưới). Cái gì AI hay quên chính là cái không nằm trong danh sách.

---

## Khung 6 lớp — làm lần lượt từ Lớp 0

### Lớp 0 · ĐẾM (làm TRƯỚC tiên)
Trước khi vẽ gì, đếm cho ra quy mô thật:
- **Ai × làm gì:** liệt kê mọi loại người dùng (khách, chủ, nhân viên, admin...) × mỗi việc họ cần làm. Đây là gốc sinh ra flow.
- **Cửa vào (entry points):** người ta vào app từ đâu? (mở web, bấm link trong email, quét QR, nhận webhook, chạy tự động theo giờ...)
- **CRUD mỗi "thứ":** mỗi đối tượng app quản lý (đơn, khách, sản phẩm...) thường cần 4 màn: **xem danh sách · xem chi tiết · tạo mới · sửa**. Đối chiếu cho đủ.

### Lớp 1 · LUỒNG + Ý ĐỊNH
Với mỗi việc chính, vẽ luồng các bước. Với mỗi bước quan trọng, ghi 1 dòng:
| Khoảnh khắc | Người dùng muốn gì | App đáp ứng sao | Họ đang lo/nghi gì |
|---|---|---|---|
| vừa bấm "thanh toán" | biết tiền đã trừ chưa | hiện trạng thái rõ ràng | "lỡ mất tiền mà không lên đơn?" |

> Luồng mà chỉ là "hộp màn nối nhau" không có cột ý định = chưa hiểu người dùng.

### Lớp 2 · MÀN HÌNH
- Liệt kê mọi màn (từ các node trong luồng + CRUD ở Lớp 0).
- **Trang hay lớp nổi?** Cần URL / nút Back / share được → **trang riêng**. Giữ nguyên màn nền, làm nhanh 1 việc → **drawer/modal**. Chỉ báo kết quả → **toast** (thông báo nhỏ). Xác nhận việc nguy hiểm → **modal chặn**.
- **Mỗi màn có nhiều trạng thái:** đừng chỉ vẽ lúc "có dữ liệu đẹp". Vẽ cả: rỗng (chưa có gì) · đang tải · lỗi · không có quyền · mất mạng · dữ liệu thiếu một nửa.

### Lớp 3 · TỪNG NÚT / Ô NHẬP
Quét cạn từng màn: mỗi nút, ô nhập, link, toggle — nó làm gì? khi nào bật/tắt/ẩn? bấm vào mở ra cái gì?

### Lớp 4 · ĐIỀU KIỆN NGHIỆM THU
Mỗi chỗ có nhánh rẽ, viết theo mẫu **Khi... Thì...**: ít nhất 1 trường hợp chạy đúng + 1 trường hợp trục trặc.
> Ví dụ: *Khi* nhập sai mã giảm giá *thì* hiện lỗi ngay dưới ô, không cho bấm tiếp.

### Lớp 5 · TRƯỜNG HỢP BIÊN (taxonomy 10 nhóm — soi đủ 10)
Đây là nơi app hay sập mà nocode không ngờ. Soi từng nhóm:
1. **Rỗng** — chưa có dữ liệu
2. **Đang tải** — chờ lâu thì sao
3. **Lỗi** — server/API trả lỗi
4. **Quyền** — không được phép làm
5. **Biên** — số quá to/quá nhỏ, tên quá dài
6. **Trùng / bấm lại** — bấm 2 lần, gửi trùng
7. **Timeout** — quá lâu không phản hồi
8. **Mạng** — rớt mạng giữa chừng
9. **Input bẩn** — nhập emoji, ký tự lạ, dán cả đoạn văn
10. **Đồng thời** — 2 người sửa cùng lúc

---

## Bước phản biện (đừng bỏ)

Sau khi có bản nháp, AI **tự đóng vai người khó tính** soi lại: luồng nào cụt? màn nào thiếu trạng thái? trường hợp biên nào chưa xử? quyền ai bị hở? Nêu **ít nhất 3 lỗ hổng** → bổ sung spec → lặp tới khi hết lỗ. (Chưa build nên sửa còn rẻ.)

## Kết quả xuất ra

Một bản UX spec gồm: bảng Ai×Việc · sơ đồ luồng + cột ý định · danh sách màn (kèm trạng thái) · từng nút/ô làm gì · điều kiện nghiệm thu · danh sách trường hợp biên đã xử lý. Giữ lại làm "hợp đồng" khi build — AI hay bạn cứ bám theo.

---

## KHÔNG làm

- ❌ Bỏ qua Lớp 0 (đếm) rồi nhảy thẳng vào vẽ → chắc chắn sót màn/case.
- ❌ Luồng không có cột "ý định" — quay lại luật 1.
- ❌ Chốt công nghệ / database / tên file — đó là việc của [`kry-system`](../kry-system/SKILL.md), không phải ở đây.
- ❌ Viết code — đây mới là bản vẽ hành vi.
- ❌ Tự bịa quy tắc khi ý tưởng còn mơ hồ → làm rõ ý tưởng trước đã.

---

*Skill trong bộ [kry-skills](https://github.com/kryphan/kry-skills). Cặp bài trùng: dùng `kry-ux` (hành vi) → rồi `kry-system` (nền) → rồi mới build. Chúc build vui 🙂*
