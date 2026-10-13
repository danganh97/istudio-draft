# iChat — Chat nội bộ kiểu Microsoft Teams

*Tóm tắt ngắn, dễ đọc cho người không rành kỹ thuật. Chi tiết đầy đủ hơn xem [SCREEN_FUNCTION_BREAKDOWN.md](SCREEN_FUNCTION_BREAKDOWN.md).*

**Chú thích:** ✅ Đã làm xong trong bản demo · 🟡 Có rồi nhưng chưa hoàn chỉnh · ⬜ Chưa làm, mới là ý tưởng

---

## Tóm tắt 1 phút

Đây là app chat nội bộ riêng (không phải Microsoft Teams thật, chỉ làm theo phong cách Teams), để studio trao đổi theo kênh dự án/team, họp video nhanh, và kết nối ngược lại với MGR. Chạy được thật trên máy chủ (có server, có lưu tin nhắn), nhưng còn **2 vấn đề lớn**: dữ liệu lưu bằng file đơn giản (chưa phải cơ sở dữ liệu thật) và đang là **một hệ thống nghỉ phép riêng**, tách biệt với bảng công của MGR.

---

## 1. Khung giao diện chung
- ✅ Thanh điều hướng: Hoạt động, Trò chuyện riêng, Kênh nhóm, Lịch họp, Tệp chia sẻ
- ✅ Tìm kiếm toàn bộ tin nhắn/thành viên/tệp (phím tắt Ctrl+K)
- ✅ Nút "Họp ngay" ở đầu trang
- ✅ Đổi tài khoản demo, xem ai đang online
- ✅ Link mở nhanh sang MGR (iWork)
- ⬜ Đăng nhập thật, mã hóa tin nhắn

## 2. Kênh & Nhóm chat
- ✅ Kênh theo 3 nhóm: Công ty & nội bộ, Dự án đang chạy, Team kỹ thuật/phòng ban
- ✅ Thu gọn/mở từng nhóm kênh
- ✅ Tạo kênh mới
- 🟡 Đánh dấu kênh yêu thích, xem danh sách thành viên kênh (nút có, cần hoàn thiện)
- ⬜ Tự tạo kênh riêng khi MGR tạo job mới (đồng bộ 2 chiều)
- ⬜ Khóa kênh, phân quyền kênh riêng tư

## 3. Chat riêng 1-1
- ✅ Danh sách người để chat riêng
- 🟡 Gửi/nhận tin nhắn riêng (khung đã có, cần làm rõ tách biệt với chat nhóm)
- ⬜ Hiện trạng thái "đang nhập", "đã xem"

## 4. Khung chat chính (trong từng kênh)
- ✅ Soạn tin: chữ đậm/nghiêng, chèn mã, gắn tên người (@mention), emoji
- ✅ Trả lời có trích tin gốc, sửa tin, thu hồi/xóa tin
- ✅ Thả cảm xúc (👍❤️😆😮😢👏)
- ✅ Ghim tin quan trọng, xem lại danh sách tin đã ghim
- ✅ Gửi kèm ảnh/tệp, xem ảnh phóng to
- ✅ So sánh ảnh render trước/sau (kéo thanh trượt)
- ✅ Tạo bình chọn (poll), chọn 1 hoặc nhiều đáp án
- ✅ Tạo phòng họp video ngay trong khung chat (Meet Now)
- ✅ Tạo công việc trong MGR ngay từ 1 tin nhắn *(hiện chỉ đăng tin giả, chưa thật sự ghi vào MGR)*
- ✅ Bot hệ thống tự đăng tin khi MGR gửi báo (webhook)
- ⬜ Trả lời lồng nhiều cấp (thread nhiều tầng)
- ⬜ Tự xóa tin theo chính sách, mã hóa đầu-cuối

## 5. Các tab trong kênh
- ✅ Tab "Tệp": xem tất cả ảnh/tệp đã chia sẻ trong kênh
- 🟡 Tab "iWork Tasks": có khung hiển thị, nhưng chưa lấy dữ liệu task thật từ MGR
- ✅ Tab "Giới thiệu": mô tả kênh, số thành viên
- ⬜ Đồng bộ task 2 chiều thật với MGR

## 6. Hoạt động, Lịch họp, Tệp (thanh bên)
- 🟡 Bản tin "Hoạt động" (nhắc tên, thả cảm xúc...) — hiện là dữ liệu mẫu tĩnh, chưa nối dữ liệu thật
- ✅ Bắt đầu họp ngay; 🟡 lên lịch họp cho tương lai (giao diện có, chưa dùng được đầy đủ)
- ✅ Xem tất cả tệp đã chia sẻ toàn hệ thống
- ⬜ Nhắc lịch họp qua email/thông báo trước giờ họp

## 7. Nghỉ phép / báo bận (trong iChat)
- ✅ Nộp đơn nghỉ phép/báo bận, Leader/Admin duyệt
- ✅ Tự đăng thông báo đơn mới vào kênh "Thông báo văn phòng"
- ⬜ **Gộp chung 1 nguồn dữ liệu nghỉ phép với MGR** — hiện đây và mục nghỉ phép trong MGR là 2 hệ thống riêng, nhập 1 nơi không thấy ở nơi khác

## 8. Kết nối với MGR
- ✅ MGR gửi thông báo sang iChat khi có việc mới/trễ hạn (giả lập)
- ⬜ iChat tạo task thật trong MGR khi dùng "tạo task từ tin nhắn"
- ⬜ Đăng nhập 1 lần dùng chung cho cả 2 app
- 🟡 Nhúng MGR làm tab trong Microsoft Teams thật (có hướng dẫn, chưa kiểm thử thật)

---

## Việc lớn còn thiếu (ảnh hưởng toàn hệ thống)

1. **Lưu dữ liệu bằng file JSON đơn giản**, chưa phải cơ sở dữ liệu thật — rủi ro mất/ghi đè dữ liệu khi nhiều người dùng cùng lúc.
2. **2 hệ thống nghỉ phép khác nhau** (iChat và MGR) — cần quyết định dùng chung 1 nơi.
3. **Nút "tạo task từ tin nhắn" chưa nối thật** với dữ liệu MGR, mới chỉ đăng tin nhắn giả.
4. **Chưa có đăng nhập thật** và chưa dùng chung tài khoản với MGR.

> Giao diện và các tính năng chat (nhắn, ghim, bình chọn, họp video...) coi như đã "thiết kế xong và chạy được". Việc còn lại chủ yếu là **nối dữ liệu thật** giữa iChat và MGR, và chuyển sang lưu trữ bền vững hơn.
