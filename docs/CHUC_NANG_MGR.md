# i8studio MGR — Hệ thống quản lý sản xuất & dự án

*Tóm tắt ngắn, dễ đọc cho người không rành kỹ thuật. Chi tiết đầy đủ hơn xem [SCREEN_FUNCTION_BREAKDOWN.md](SCREEN_FUNCTION_BREAKDOWN.md).*

**Chú thích:** ✅ Đã làm xong trong bản demo · 🟡 Có rồi nhưng chưa hoàn chỉnh · ⬜ Chưa làm, mới là ý tưởng

---

## Tóm tắt 1 phút

Đây là app thay Redmine, để quản lý toàn bộ quy trình làm job 3D: từ lúc nhận dự án → giao việc → làm → kiểm tra chất lượng → giao khách → tính công, tính điểm KPI.

Gồm **11 màn hình chính**. Phần lớn nghiệp vụ đã được dựng xong trong bản demo (chạy thử trên máy, chưa có server thật). Việc còn thiếu lớn nhất không nằm ở từng màn hình, mà ở phần **nền tảng**: chưa có server/database thật, nên dữ liệu chưa dùng chung được giữa nhiều người, và ai mở trình duyệt cũng sửa được (xem mục "Việc lớn còn thiếu" cuối trang).

---

## 1. Đăng nhập (`#/login`)
- ✅ Đăng nhập demo, chọn nhanh vai trò để thử
- ⬜ Đăng nhập thật (mật khẩu/tài khoản công ty), quên mật khẩu thật

## 2. Dashboard điều hành (`#/dashboard`) — *chỉ Ban giám đốc, Admin*
- ✅ Xem tổng quan toàn công ty: job đang chạy, task trễ hạn, điểm/giờ theo team
- ✅ Xem biểu đồ hoặc bảng, lọc theo team/thời gian
- 🟡 Xuất báo cáo PDF/Excel (có script chạy tay, chưa có nút bấm)
- ⬜ So sánh hiệu suất giữa các tháng

## 3. Danh sách & Tạo dự án (`#/projects`) — *CS, Leader, BOD*
- ✅ Xem danh sách job, lọc theo trạng thái/team/khách
- ✅ Tạo job mới theo 3 mẫu có sẵn: Ngoại thất / Nội thất / Movie
- ✅ Tự đặt mã Job, tự tạo đường dẫn thư mục mạng
- ✅ Gán người phụ trách (CS, Leader, team), đặt ngân sách điểm
- ✅ Job cha có nhiều job con dùng chung mã
- ✅ Duyệt báo giá trước khi chạy sản xuất
- ⬜ Luồng duyệt báo giá nhiều cấp, lưu lịch sử giá
- ⬜ Gắn hợp đồng / điều khoản thanh toán vào job

## 4. Chi tiết dự án — Kế hoạch sản xuất (`#/project/{id}`)
- ✅ Xem đầy đủ thông tin job, sửa thông tin (người có quyền)
- ✅ Chia job thành các giai đoạn (Model → 1st → Final), mỗi giai đoạn có nhiều góc ảnh (view)
- ✅ Thêm task, gán người làm, hạn, điểm/giờ
- ✅ Xem kế hoạch dạng bảng hoặc dạng Gantt (biểu đồ thời gian)
- ✅ Duyệt xong 1 giai đoạn → tự mở giai đoạn kế tiếp
- ✅ Nhập phản hồi sửa của khách (CB) → tự tạo task sửa, phân loại miễn phí/tính phí
- ✅ Nhận khiếu nại sau khi giao file cuối (Claim), phân loại lỗi nội bộ/tính phí
- ✅ Cảnh báo khi vượt ngân sách điểm
- ⬜ Đính kèm file brief/thiết kế trực tiếp vào job (hiện chỉ có link thư mục)
- ⬜ Theo dõi hoá đơn/thanh toán theo tiến độ

## 5. Việc của tôi (`#/work`) — *Leader, Staff*
- ✅ Xem task được giao
- ✅ Bấm giờ làm (chỉ chạy 1 đồng hồ/người cùng lúc)
- ✅ Nhập giờ làm thủ công khi quên bấm giờ
- ✅ Bàn giao check (không cho bàn giao nếu còn lỗi chưa sửa)
- ✅ Mở chi tiết task để bình luận, xem lịch sử, xem lỗi
- ⬜ Lịch làm việc cá nhân/to-do riêng ngoài task hệ thống

## 6. Kiểm tra chất lượng — QC (`#/qc`) — *Leader, QA, BOD*
- ✅ Xem ảnh render, zoom to/thu nhỏ
- ✅ Ghim pin đánh dấu lỗi trực tiếp trên ảnh, ghi mô tả
- ✅ Yêu cầu sửa (phải có ít nhất 1 lỗi) hoặc Duyệt đạt (phải hết lỗi)
- ✅ Bình luận trong khi kiểm tra
- ⬜ So sánh ảnh trước/sau khi sửa (slider before/after)
- ⬜ Xem file 3D/video, không chỉ ảnh tĩnh

## 7. Bản đồ công việc — Kanban (`#/kanban`)
- ✅ Kéo-thả task qua các cột trạng thái
- ✅ 4 cách nhìn: theo dự án / theo người / theo team / nhân sự đang rảnh
- ✅ Lọc theo team, dự án, người, "chỉ việc của tôi"
- ✅ Duyệt mượn người giữa các team ngay trên bảng
- ⬜ Xem theo lịch tuần (không chỉ theo cột trạng thái)

## 8. KPI & Điểm (`#/kpi`) — *BOD, CS, Leader, QA (Staff không xem)*
- ✅ Bảng điểm: điểm làm cho team mình vs điểm hỗ trợ team khác
- ✅ Tỷ lệ đúng hạn, số lỗi trung bình, số vòng sửa mỗi người
- ✅ Xuất báo cáo, xem chi tiết từng người
- ⬜ Bảng xếp hạng giữa các nhân sự
- ⬜ Biểu đồ xu hướng điểm qua nhiều tháng

## 9. Bảng công & Nghỉ phép (`#/time`)
- ✅ Tự tính giờ hành chính/tăng ca (thường, đêm, ngày nghỉ, ngày lễ) từ giờ bấm
- ✅ Nộp đơn nghỉ phép, Leader duyệt
- ✅ Yêu cầu sửa giờ chấm công, Leader duyệt
- ✅ Khóa bảng công theo tháng, xuất CSV theo pháp nhân
- 🟡 Cảnh báo hết quỹ phép năm (có số liệu, UI cảnh báo chưa rõ)
- ⬜ Tính lương

## 10. Quản trị hệ thống (`#/admin`) — *BOD, Admin*
- ✅ Cấu hình: loại task & điểm, team & chỉ tiêu, mã khách hàng, ca làm/giờ tăng ca/ngày lễ
- ✅ Quản lý người dùng (xem), đổi logo/thương hiệu
- ✅ Xem nhật ký thay đổi quan trọng (audit log)
- ✅ Xóa vĩnh viễn job/task (chỉ Admin)
- ✅ Cấu hình kết nối iChat & Microsoft Teams, gửi thử, báo động deadline hôm nay
- ✅ Reset dữ liệu demo về ban đầu
- ⬜ Phân quyền chi tiết theo từng người (ngoài 7 vai trò có sẵn)
- ⬜ Sao lưu/khôi phục dữ liệu thật

## 11. Giao diện điện thoại (`#/mobile`)
- ✅ Xem task, bấm giờ, mở chat, nộp phép, xem thông báo — bản rút gọn
- ⬜ App di động thật (cài trên máy, dùng được khi mất mạng)

---

## Việc lớn còn thiếu (ảnh hưởng toàn hệ thống)

1. **Chưa có server + cơ sở dữ liệu thật.** Hiện dữ liệu lưu trên trình duyệt từng máy (localStorage) — nhiều người không dùng chung được, xóa cache là mất dữ liệu.
2. **Phân quyền mới chặn ở giao diện.** Ai mở công cụ lập trình trong trình duyệt cũng sửa được dữ liệu/quyền. Cần làm lại ở phía server.
3. **Chưa có đăng nhập thật**, mật khẩu hiện chỉ kiểm tra giả ở máy người dùng.
4. **Thông báo mới là giả lập**, chưa gửi email/thông báo điện thoại thật.
5. **Chưa có quy trình kiểm thử tự động (CI)** trước khi đưa bản mới lên trang chạy thật.

> Ba mục 1–3 là nền tảng bắt buộc phải làm trước khi đưa vào dùng thật — các màn hình/nghiệp vụ ở trên coi như đã "thiết kế xong", chỉ cần viết lại trên nền server thật.
