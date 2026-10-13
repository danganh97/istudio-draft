# Ước lượng triển khai bản thật (từ demo → sản phẩm dùng được)

*Ước lượng sơ bộ, dùng để trao đổi với đơn vị tư vấn/đội dev — không phải báo giá chính thức.*

## Cơ sở ước lượng

- Nghiệp vụ & giao diện coi như đã **thiết kế xong** qua bản demo (xem [CHUC_NANG_MGR.md](CHUC_NANG_MGR.md), [CHUC_NANG_ICHAT.md](CHUC_NANG_ICHAT.md)) — việc còn lại là **viết lại trên nền server thật** + nối 2 app với nhau.
- Giả định nhóm dev gồm **1 Backend + 1 Frontend + 1 ngày/tuần QA**, có thể làm song song.
- Không tính thời gian chờ duyệt/phản hồi nghiệp vụ từ phía công ty (chốt yêu cầu, UAT) — cộng thêm tuỳ thực tế.
- Đơn vị: **tuần người** (1 tuần người = 1 người làm full-time 1 tuần).

## Ước lượng theo giai đoạn

| Giai đoạn | Nội dung | Tuần người | Thời gian lịch (2 dev) |
|---|---|---|---|
| 1. Nền tảng | Server + database thật, đăng nhập/phân quyền thật (BE), dựng khung API | 4–6 | 2–3 tuần |
| 2. Chuyển app MGR | Viết lại 11 màn hình trên nền server thật (giữ nguyên nghiệp vụ demo), nối API | 10–14 | 5–7 tuần |
| 3. Chuyển app iChat | Chuyển file JSON → database, đăng nhập thật, giữ nguyên tính năng chat đã có | 4–5 | 2–3 tuần |
| 4. Hợp nhất & tích hợp | Gộp 1 nguồn dữ liệu nghỉ phép, nối "tạo task từ tin nhắn" vào MGR thật, SSO 1 lần đăng nhập cho 2 app, webhook thông báo thật | 3–4 | 1.5–2 tuần |
| 5. Kiểm thử & vá lỗi (QA/UAT) | Kiểm thử tự động (CI), kiểm thử tay theo vai trò, sửa lỗi phát sinh | 3–4 | 2–3 tuần (chạy song song cuối GĐ 2–4) |
| 6. Triển khai & bàn giao | Lên server thật, cấu hình domain/SSL, hướng dẫn sử dụng, theo dõi 2 tuần đầu | 1–2 | 1 tuần |
| **Tổng** | | **~25–35 tuần người** | **~12–16 tuần lịch (≈ 3–4 tháng)** |

## Theo quy mô team

| Quy mô team | Thời gian hoàn thành (ước lượng) |
|---|---|
| 1 dev full-stack | ~6–8 tháng |
| 2 dev (1 BE + 1 FE) | ~3–4 tháng |
| 3–4 dev (2 BE + 1–2 FE + 1 QA) | ~2–2.5 tháng |

## Yếu tố có thể làm tăng thời gian

- Yêu cầu thêm tính năng mới ngoài danh sách đã demo (vd: tính lương, app mobile thật, mã hoá tin nhắn).
- Tích hợp SSO Microsoft 365 / LDAP thật nếu công ty yêu cầu (cộng thêm ~1–2 tuần).
- Số lượng dữ liệu/người dùng lớn cần tối ưu hiệu năng sớm.
- Thời gian chờ công ty chốt nghiệp vụ còn mở (mục ⬜/🟡 trong 2 file chức năng).

## Kết luận ngắn

Với 1 đội **2 người (1 Backend + 1 Frontend)** làm full-time, có thể đưa bản demo này thành sản phẩm dùng thật trong khoảng **3–4 tháng**, với điều kiện giữ đúng phạm vi nghiệp vụ đã có trong demo và không phát sinh yêu cầu lớn mới.
