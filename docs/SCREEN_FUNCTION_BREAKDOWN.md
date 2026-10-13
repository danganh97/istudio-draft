# i8studio MGR — Phân rã Màn hình & Chức năng (toàn hệ thống)

> Tài liệu này liệt kê **toàn bộ màn hình và chức năng cần có** cho một bản sản phẩm thật, tổng hợp từ:
> - `TAI_LIEU_HUONG_DAN_SU_DUNG_i8studio_MGR.pdf` / `document_guide.html` (tài liệu nghiệp vụ)
> - `README.md`, `docs/BAO_CAO_TONG_QUAN.md` (báo cáo kiến trúc)
> - Code đã triển khai trong bản demo (`js/views/*.js`, `js/app.js`, `js/store.js`, `js/logic_*.js`, `ichat/`)
>
> Hệ thống gồm **2 ứng dụng**: **(A) i8studio MGR** — quản lý sản xuất/dự án (thay Redmine) và **(B) iChat** — chat nội bộ kiểu Microsoft Teams. Mỗi mục dưới đây đánh dấu trạng thái: ✅ đã có trong demo · 🟡 có một phần / cần hoàn thiện cho bản thật · ⬜ chưa làm, chỉ có trong tài liệu/ý tưởng.

---

## A. ỨNG DỤNG QUẢN LÝ (i8studio MGR)

### A0. Nền tảng chung (áp dụng mọi màn hình)

| # | Chức năng | Trạng thái | Ghi chú |
|---|---|---|---|
| A0.1 | Đăng nhập demo theo tài khoản mẫu, chọn nhanh vai trò | ✅ | `js/views/login.js`; cần thay bằng đăng nhập thật (SSO/LDAP/tài khoản nội bộ) |
| A0.2 | Đăng nhập thật (mật khẩu/SSO Microsoft 365) | ⬜ | Nêu trong rủi ro #3 của `BAO_CAO_TONG_QUAN.md` |
| A0.3 | Phân quyền theo vai trò: BOD, Báo giá, CS, Leader, QA, Staff, Admin | ✅ | Ma trận `S.can()` (`js/store.js`), `ACCESS`/`NAV_HIDE` (`js/views/common.js`) |
| A0.4 | Phân quyền dữ liệu theo phạm vi (chỉ thấy job của team/PM mình) | ✅ | `S.visibleProjects`, `S.canSeeProject` |
| A0.5 | Phân quyền thực thi ở server (không chỉ ở UI) | ⬜ | Rủi ro #2 — bắt buộc cho bản thật |
| A0.6 | Sidebar điều hướng theo vai trò + thu gọn | ✅ | `js/views/shell.js` |
| A0.7 | Topbar: tìm kiếm nhanh (palette), chuyển đổi tài khoản demo, chuông thông báo, hồ sơ cá nhân | ✅ | `shell.js` (`search-go`, `palette-*`, `notif-*`, `profile-*`) |
| A0.8 | Bảng điều khiển thông báo (drawer) theo nhóm: QC, Sửa lỗi, Deadline, Hành chính, Dự án, Công việc | ✅ | `shell.js` hàm `notifDrawer` |
| A0.9 | Đẩy thông báo ra iChat / Microsoft Teams webhook | ✅ (giả lập) | `S.sendToIChat`, `S.sendToTeams` trong `store.js`; cần endpoint thật khi có backend |
| A0.10 | Theme sáng / tối / tự động theo hệ thống | ✅ | `app.js` `applyTheme`, cấu hình trong hồ sơ |
| A0.11 | Chọn ngôn ngữ (VI mặc định) | 🟡 | Có cờ `spf-lang` trong hồ sơ, nội dung UI hiện chỉ có tiếng Việt |
| A0.12 | Giao diện rút gọn cho điện thoại | ✅ | `js/views/mobile.js`, route `#/mobile` |
| A0.13 | Audit log (nhật ký biến động) | ✅ | `S.auditLog`, tab "Nhật ký biến động" trong Quản trị |
| A0.14 | Toast / modal dùng chung | ✅ | `js/util.js` |
| A0.15 | Lưu trữ dữ liệu thật (backend + DB) thay localStorage | ⬜ | Rủi ro #1 — nền tảng để mọi mục trên hoạt động nhiều người dùng đồng thời |

### A1. Đăng nhập & chọn vai trò demo — `#/login`

- ✅ Form đăng nhập giả lập + nút "tài khoản demo" theo từng vai trò (`login-demo-account`)
- ✅ "Quên mật khẩu" (giả lập, chỉ demo) (`login-forgot`)
- ⬜ Đăng nhập thật, quên mật khẩu thật, khóa tài khoản sau N lần sai

### A2. Dashboard điều hành (BOD/Admin) — `#/dashboard`

Truy cập: chỉ **BOD, Admin**.

| # | Chức năng | Trạng thái |
|---|---|---|
| A2.1 | KPI tổng quan toàn công ty (số job đang chạy, task quá hạn, điểm/giờ theo team) | ✅ |
| A2.2 | Chuyển đổi hiển thị biểu đồ ↔ bảng (`dash-mode`) | ✅ |
| A2.3 | Theo dõi tiến độ job, cảnh báo job trễ/vượt điểm | ✅ |
| A2.4 | Điều hướng nhanh tới job/người (`go-project`, `go-people-view`) | ✅ |
| A2.5 | Lọc theo team, theo khoảng thời gian | ✅ |
| A2.6 | Xuất báo cáo PDF/Excel cho BOD | 🟡 | có `scripts/export_report_pdf.js` dạng script, chưa có nút trong UI |
| A2.7 | So sánh hiệu suất giữa các kỳ (tháng trước/tháng này) | ⬜ |

### A3. Danh sách & Khởi tạo dự án (CS/Leader/BOD) — `#/projects`

Truy cập: BOD, Báo giá, CS, Leader, QA, Admin (Staff không vào).

| # | Chức năng | Trạng thái |
|---|---|---|
| A3.1 | Danh sách dự án dạng card + bảng, lọc theo trạng thái/team/khách/từ khóa | ✅ |
| A3.2 | Khởi tạo dự án từ 3 template: Ngoại thất, Nội thất, Movie | ✅ | `S.TEMPLATES` |
| A3.3 | Tự sinh mã Job chuẩn `SốJob_MãKhách_TênVI_TênJP` | ✅ | `S.makeCode`, `S.nextNo` |
| A3.4 | Tự sinh đường dẫn thư mục mạng NAS theo mã job | ✅ | `job.nas` |
| A3.5 | Chọn dãy số job: dùng chung hoặc riêng theo khách | ✅ | Quản trị → Khách & dãy số |
| A3.6 | Gán CS phụ trách + Co-PM, Leader Model/Visual, team phụ trách | ✅ | `np-team`, `createdBy` |
| A3.7 | Thiết lập điểm ngân sách (budget) dự án + cân lại điểm khi đổi gói | ✅ | `np-pkg-op`, `np-rebalance`, `S.redistributeProjectPoints` |
| A3.8 | Job cha – dự án con (nhiều hạng mục dùng chung mã Job + mã khách) | ✅ | `parentId`, `proj-add-child`, `proj-children-toggle` |
| A3.9 | Trạng thái dự án: Chờ duyệt báo giá → Ready → Đang chạy → Tạm dừng → Đã giao → Đã đóng → Lưu trữ, với ràng buộc chuyển trạng thái | ✅ | `S.setProjectStatus`, `S.statusChangeError` |
| A3.10 | Phê duyệt báo giá (chuyển từ "Chờ duyệt báo giá" sang "Ready") | ✅ | `proj-approve-quote` |
| A3.11 | Tìm kiếm & lọc theo PM/CS phụ trách (担当) | ✅ |
| A3.12 | Đính kèm ảnh đại diện dự án | ✅ | `np-img` |
| A3.13 | Quy trình duyệt báo giá có luồng phê duyệt nhiều cấp, lưu lịch sử báo giá | ⬜ |
| A3.14 | Gắn hợp đồng/điều khoản thanh toán vào dự án | ⬜ |

### A4. Chi tiết dự án — Kế hoạch sản xuất Waterfall — `#/project/{id}`

Truy cập: tất cả vai trò (trừ giới hạn phạm vi thấy được).

| # | Chức năng | Trạng thái |
|---|---|---|
| A4.1 | Header dự án: mã job đầy đủ, khách, team, CS/Co-PM, Leader, link NAS copy nhanh | ✅ |
| A4.2 | Chỉnh sửa thông tin job (chỉ người tạo/quản lý) | ✅ | `proj-edit` |
| A4.3 | Cấu trúc giai đoạn (Phase): Model → 1st → Final, mỗi phase có nhiều góc View | ✅ |
| A4.4 | Thêm/xoá góc View (camera) cho dự án, kể cả view nhập tay | ✅ | `nt-view-add`, `np-view-add/del` |
| A4.5 | Thêm task mới theo loại/giai đoạn/view, gán người, hạn, điểm/giờ | ✅ | `task-new` |
| A4.6 | 2 chế độ xem kế hoạch: bảng (table) / Gantt timeline | ✅ | `wf-mode`, `wf: 'table'` trong `V.ui` |
| A4.7 | Lọc/tìm task trong Waterfall (theo người, trạng thái, từ khóa) | ✅ | `wf-filter`, `wf-search`, `wf-filter-reset` |
| A4.8 | Đặt mốc baseline (so sánh tiến độ thực tế với kế hoạch gốc) | ✅ | `wf-baseline` |
| A4.9 | Mở khóa phase kế tiếp sớm (trước khi phase trước hoàn thành 100%) | ✅ | `S.can('manageTask')`/nút Mở khóa sớm trong `taskActions` |
| A4.10 | Duyệt hoàn thành Phase (Leader) → tự mở Phase kế tiếp | ✅ | `phase-approve` |
| A4.11 | Đặt/điều chỉnh hạn chót Phase | ✅ | `phase-due` |
| A4.12 | Bàn giao khách (Đã giao) khi Phase Final hoàn thành | ✅ | `phase-deliver` |
| A4.13 | Nhập Check-back (CB) khách: ngày phản hồi + danh sách việc sửa, tự sinh task `fix cb YYMMDD – …` | ✅ | `cb-new`, `cb-row-add`, `S` logic trong `logic_task.js` |
| A4.14 | Phân loại CB: Trong phạm vi / Phát sinh tính phí (cộng điểm ngân sách) | ✅ |
| A4.15 | Claim khiếu nại sau Final: Lỗi nội bộ (0 điểm KPI) / Tính phí (cộng điểm), cờ GẤP | ✅ | `claim-new` |
| A4.16 | Cân lại điểm giữa các task trong dự án khi đổi budget | ✅ | `proj-rebalance-points` |
| A4.17 | Cảnh báo vượt điểm ngân sách dự án | ✅ | `S.checkBudgetOverflow` |
| A4.18 | Ghi chú nội bộ dự án (ẩn/hiện) | ✅ | `proj-note-toggle` |
| A4.19 | Sửa đường dẫn server theo từng task | ✅ | `job-edit-server`, `task-edit-server` |
| A4.20 | Lịch sử thay đổi của dự án/task (ai sửa gì, khi nào) | ✅ (one-line log) | `S.log`; cần UI xem lịch sử đầy đủ hơn cho bản thật |
| A4.21 | Đính kèm file thiết kế/brief khách trực tiếp vào dự án (không chỉ qua link NAS) | ⬜ |
| A4.22 | Quản lý hợp đồng/điều khoản thanh toán theo tiến độ (invoice theo % hoàn thành) | ⬜ |

### A5. Việc của tôi / Workspace cá nhân — `#/work`

Truy cập: Leader, Staff, BOD.

| # | Chức năng | Trạng thái |
|---|---|---|
| A5.1 | Danh sách task được giao, nhóm theo trạng thái | ✅ |
| A5.2 | Đồng hồ bấm giờ (Live Timer) đơn nhiệm: chỉ 1 timer chạy/người, chuyển task tự tạm dừng task cũ | ✅ | `S.timingTask`, action `tr` (start/pause/handover/resume/clientPass) |
| A5.3 | Nhập giờ làm thủ công (quên bấm giờ) | ✅ | `work-manual-time` |
| A5.4 | Bàn giao check — chặn nếu còn pin lỗi chưa sửa | ✅ | `logic_task.js` |
| A5.5 | Copy nhanh đường dẫn server (NAS) của task | ✅ | `copy-server-link` |
| A5.6 | Mở modal chi tiết task (bình luận, lịch sử, pin lỗi) | ✅ | `task-open` |
| A5.7 | Đặt/xem trạng thái pin lỗi của task | ✅ | `pin-set` |
| A5.8 | Khách yêu cầu sửa / Khách đã phản hồi (đóng băng-chờ khách) | ✅ | trạng thái `Chờ khách`, action `tr: resume/clientPass` |
| A5.9 | Thông báo real-time khi được giao task mới | ✅ (qua notif + webhook) |
| A5.10 | Lịch làm việc cá nhân / to-do ngoài task hệ thống | ⬜ |

### A6. Kiểm soát chất lượng — QC Pin & Duyệt task — `#/qc`

Truy cập: BOD, Leader, QA.

| # | Chức năng | Trạng thái |
|---|---|---|
| A6.1 | Danh sách task đang "Chờ check", theo team/người | ✅ |
| A6.2 | Lightbox xem ảnh render full màn hình, zoom in/out/fit/reset | ✅ | `qc-lightbox`, `qc-zoom-*` |
| A6.3 | Ghim pin lỗi trực tiếp lên ảnh (tọa độ) + mô tả lỗi | ✅ | `qc-add-pin`, `pin-set`, `qc-tool` |
| A6.4 | Chuyển đổi công cụ ghim pin / chế độ xem (`qc-mode`) | ✅ |
| A6.5 | Yêu cầu sửa (bắt buộc ≥1 pin lỗi), tự tăng `fixRounds` | ✅ | `qc-fix` |
| A6.6 | Duyệt Đạt (Pass) — chỉ khi mọi pin đã "Đã sửa" | ✅ | `qc-pass` |
| A6.7 | Bình luận trên task trong khi QC | ✅ | `task-comment`, `tr-reason` |
| A6.8 | Duyệt Phase ngay từ màn QC khi đủ điều kiện | ✅ | `phase-approve` |
| A6.9 | So sánh ảnh trước/sau khi sửa lỗi (before/after slider) | ⬜ (có ý tưởng tương tự ở iChat `render_compare`, chưa có ở MGR) |
| A6.10 | Soi file 3D/video thay vì chỉ ảnh tĩnh | ⬜ |

### A7. Bản đồ công việc Kanban — `#/kanban`

Truy cập: tất cả vai trò.

| # | Chức năng | Trạng thái |
|---|---|---|
| A7.1 | 8 cột theo trạng thái task, kéo-thả đổi trạng thái | ✅ | `dragstart/dragover/drop` trong `app.js` |
| A7.2 | 4 góc nhìn khác (theo dự án / theo người / theo team / "Nhân sự rảnh" bench) | ✅ | `kb-view`, `go-people-view('bench')` |
| A7.3 | Lọc theo team, dự án, người phụ trách, "chỉ việc của tôi" | ✅ | `kb-team`, `V.ui.kanban` |
| A7.4 | Chế độ xem rộng (wide) | ✅ | `kb-toggle-wide` |
| A7.5 | Mở nhanh task / copy link server từ card | ✅ | `task-open`, `copy-server-link` |
| A7.6 | Duyệt Đạt (QC) ngay trên card Kanban | ✅ | `qc-pass` |
| A7.7 | Quyết định mượn người chéo team (chấp nhận/từ chối) | ✅ | `borrow-dec` |
| A7.8 | Mở modal khách yêu cầu sửa ngay từ card | ✅ | `client-comment-modal` |
| A7.9 | Timeline/lịch biểu trực quan theo tuần (không chỉ cột trạng thái) | ⬜ |

### A8. KPI & Bảng ma trận điểm — `#/kpi`

Truy cập: BOD, CS, Leader, QA (Staff & Báo giá không được xem).

| # | Chức năng | Trạng thái |
|---|---|---|
| A8.1 | Bảng điểm theo người: điểm Nhà (own) vs điểm Mượn (borrowed) | ✅ |
| A8.2 | Tỷ lệ đúng hạn (on-time rate) theo lần bàn giao đầu tiên | ✅ |
| A8.3 | Chỉ số chất lượng: số pin lỗi trung bình / task, số vòng sửa (fixRounds) | ✅ |
| A8.4 | Lọc theo khoảng thời gian, theo team | ✅ | `V.ui.kpi` |
| A8.5 | Xuất báo cáo KPI (CSV/Excel) | ✅ | `kpi-export` |
| A8.6 | Xem chi tiết KPI của 1 người (drill-down) | ✅ | `kpi-user` |
| A8.7 | So sánh target điểm/tháng của team với thực tế | ✅ | field `team.target` trong Quản trị |
| A8.8 | Xếp hạng / leaderboard giữa các Artist | ⬜ |
| A8.9 | Biểu đồ xu hướng KPI theo nhiều tháng | ⬜ |

### A9. Bảng công & Nghỉ phép — `#/time`

Truy cập: tất cả vai trò.

| # | Chức năng | Trạng thái |
|---|---|---|
| A9.1 | Tự động tách giờ theo mã công H/T1/T2/T3/T4 từ dữ liệu Timer | ✅ | `js/logic_time.js` |
| A9.2 | Xem bảng công theo ngày / theo tháng | ✅ | `time-day`, `time-month`, `time-today` |
| A9.3 | Thêm/xoá dòng chấm công thủ công | ✅ | `entry-add`, `entry-del` |
| A9.4 | Nộp đơn nghỉ phép (P/N1/N2/N3) | ✅ | `leave-req` |
| A9.5 | Duyệt/từ chối đơn nghỉ phép (Leader/Admin) | ✅ | `leave-dec` |
| A9.6 | Yêu cầu sửa giờ chấm công (trong hạn cho phép) + Leader duyệt | ✅ | `edit-req`, `edit-dec` |
| A9.7 | Khóa bảng công theo tháng (Admin/BOD) | ✅ | `time-lock` |
| A9.8 | Xuất CSV bảng công theo pháp nhân (entity) | ✅ | `time-export` |
| A9.9 | Xuất báo cáo tổng hợp (summary) | ✅ | `time-export-summary` |
| A9.10 | Tính quỹ phép năm còn lại, cảnh báo khi âm quỹ | 🟡 | có field quỹ phép trong Quản trị, cần UI cảnh báo rõ hơn |
| A9.11 | Tích hợp tính lương (payroll) | ⬜ |

### A10. Quản trị hệ thống — `#/admin`

Truy cập: BOD, Admin.

| Tab | Chức năng | Trạng thái |
|---|---|---|
| A10.1 Loại task & điểm | Thêm/sửa loại task theo nhóm, giờ & điểm mặc định, map tracker MGR cũ, cảnh báo trùng tracker | ✅ |
| A10.2 Team & target | Thêm team (nội bộ/ngoài), gán pháp nhân, target điểm/tháng, xem leader/số thành viên | ✅ |
| A10.3 Khách & dãy số | Thêm mã khách, chọn dãy số chung/riêng, xem số job tiếp theo | ✅ |
| A10.4 Ca làm · T2 · ngày lễ | Cấu hình ca làm việc, mốc tăng ca T2 (21h), quy định thứ 7, danh sách ngày lễ | ✅ |
| A10.5 Người dùng & vai trò | Danh sách người dùng, vai trò, team, loại Leader | ✅ (xem) / 🟡 (CRUD đầy đủ người dùng) |
| A10.6 Thương hiệu & Logo | Đổi logo, màu thương hiệu (chỉ role được cấp quyền `changeBrand`) | ✅ | `adm-brand-save`, `adm-brand-reset` |
| A10.7 Nhật ký biến động (Audit Log) | Xem lịch sử các thay đổi quan trọng | ✅ |
| A10.8 Xóa vĩnh viễn (Admin IT) | Xóa cứng dự án/task (chỉ Admin) | ✅ | `adm-del-proj`, `adm-del-task` |
| A10.9 Tích hợp iChat & Teams | Cấu hình webhook iChat/Teams, test gửi thử, nhúng iWork làm tab Teams, broadcast cảnh báo deadline hôm nay | ✅ | `adm-save-ichat/teams`, `adm-test-ichat/teams`, `broadcast-deadlines` |
| A10.10 Dữ liệu demo | Reset dữ liệu về seed ban đầu | ✅ | `adm-reset` |
| A10.11 Quản lý quyền hạn chi tiết (custom permission per user) | ⬜ |
| A10.12 Cấu hình backup/restore dữ liệu thật | ⬜ |

### A11. Giao diện điện thoại (rút gọn) — `#/mobile`

Truy cập: Leader, QA, Staff.

| # | Chức năng | Trạng thái |
|---|---|---|
| A11.1 | Danh sách task rút gọn, bấm giờ nhanh | ✅ | `tr` |
| A11.2 | Mở chat/iChat từ mobile | ✅ | `m-chat` |
| A11.3 | Nộp nghỉ phép nhanh | ✅ | `m-leave` |
| A11.4 | Xem/mở thông báo | ✅ | `notif-open` |
| A11.5 | App di động thật (PWA/offline) | ⬜ | Nêu trong câu hỏi #4 báo cáo tổng quan |

---

## B. ỨNG DỤNG CHAT NỘI BỘ (iChat — "Microsoft Teams Edition")

> Thư mục `ichat/`: Express + WebSocket server (`server.js`) + giao diện tĩnh (`public/`). Độc lập hoàn toàn với MGR, chỉ liên kết lỏng qua link mở "iWork" và webhook tạo thông báo.

### B0. Hạ tầng & kết nối

| # | Chức năng | Trạng thái | Ghi chú |
|---|---|---|---|
| B0.1 | Kết nối realtime qua WebSocket (tin nhắn mới, đổi reaction, pin, poll, online count) | ✅ | `wss` trong `server.js`, `broadcast()` |
| B0.2 | Lưu trữ bền bằng file JSON (`data/messages.json`, `leaves.json`, `meetings.json`) | ✅ | 🟡 cần chuyển sang DB thật khi scale |
| B0.3 | Trạng thái dịch vụ (health check) | ✅ | `GET /api/status` |
| B0.4 | Chuyển đổi nhanh tài khoản demo (không có đăng nhập thật) | ✅ | dropdown `#user-select` |
| B0.5 | Xác thực người dùng thật, mã hoá tin nhắn | ⬜ |
| B0.6 | Deploy Docker (có sẵn `Dockerfile`, `docker-compose.yml`, cấu hình nginx) | ✅ |

### B1. Khung giao diện chính (Teams layout)

| # | Chức năng | Trạng thái |
|---|---|---|
| B1.1 | Thanh điều hướng dọc (rail): Hoạt động, Trò chuyện, Kênh Team, Lịch họp, Tệp chia sẻ, link mở iWork | ✅ |
| B1.2 | Thanh tìm kiếm toàn cục (tin nhắn, thành viên, tệp, kênh), phím tắt Ctrl+K | ✅ | `GET /api/messages/search` |
| B1.3 | Nút "Meet Now" nhanh ở header | ✅ |
| B1.4 | Chuyển đổi tài khoản + hiển thị trạng thái online/busy/leave | ✅ |
| B1.5 | Badge số lượng người online (qua VPS) | ✅ |
| B1.6 | Link mở nhanh iWork MGR | ✅ |

### B2. Kênh & Nhóm (Teams/Channels)

| # | Chức năng | Trạng thái |
|---|---|---|
| B2.1 | Danh mục kênh theo nhóm: Công ty & nội bộ, Kênh dự án 3D đang chạy, Nhóm kỹ thuật & phòng ban | ✅ | `CHANNELS` trong `server.js` |
| B2.2 | Thu gọn/mở rộng từng nhóm kênh | ✅ | `data-toggle` |
| B2.3 | Tạo kênh trao đổi mới | ✅ (nút có, cần xác nhận luồng lưu) | `#btn-add-channel` |
| B2.4 | Badge số đơn nghỉ phép/báo bận chưa xử lý trên kênh | ✅ | `#leaves-count` |
| B2.5 | Đánh dấu kênh yêu thích (star) | 🟡 (icon có, chưa rõ lưu trạng thái) |
| B2.6 | Danh sách thành viên kênh | 🟡 | nút `btn-chan-members` có, cần kiểm tra modal |
| B2.7 | Tự tạo kênh dự án khi MGR khởi tạo job mới (đồng bộ 2 chiều) | ⬜ |
| B2.8 | Quản trị kênh: xoá/khóa kênh, phân quyền kênh riêng tư | ⬜ |

### B3. Tin nhắn 1-1 (Direct Message)

| # | Chức năng | Trạng thái |
|---|---|---|
| B3.1 | Danh sách người để chat riêng | ✅ | `sb-view-chat`, `dm-users-list` |
| B3.2 | Gửi/nhận tin nhắn 1-1 realtime | 🟡 | FE có khung, cần xác nhận API phân biệt DM với kênh (hiện API dùng chung `channel`) |
| B3.3 | Trạng thái đã xem / đang nhập (typing indicator) | ⬜ |

### B4. Khung chat theo kênh (Posts)

| # | Chức năng | Trạng thái |
|---|---|---|
| B4.1 | Soạn tin nhắn: định dạng đậm/nghiêng/code, @mention, emoji | ✅ | toolbar `composer-toolbar` |
| B4.2 | Gửi tin nhắn (Enter gửi, Shift+Enter xuống dòng) | ✅ |
| B4.3 | Trả lời (reply) có trích dẫn tin nhắn gốc | ✅ | `replyTo`, `reply-bar` |
| B4.4 | Sửa tin nhắn | ✅ | `PUT /api/messages/:id` |
| B4.5 | Thu hồi/xoá tin nhắn | ✅ | `DELETE /api/messages/:id` |
| B4.6 | Thả reaction emoji (👍❤️😆😮😢👏), bật/tắt theo người | ✅ | `POST /api/messages/:id/react` |
| B4.7 | Ghim tin nhắn quan trọng + banner "đã ghim" + xem danh sách đã ghim | ✅ | `POST /api/messages/:id/pin`, modal `modal-pinned-list` |
| B4.8 | Đính kèm ảnh/tệp (kéo-thả hoặc chọn), xem trước | ✅ | modal `modal-attach` |
| B4.9 | So sánh ảnh render Before/After (thanh trượt) | ✅ | `attachment.type === 'render_compare'`, modal `modal-compare` |
| B4.10 | Tạo & bình chọn (Poll), đơn/đa lựa chọn | ✅ | `POST /api/polls`, `POST /api/messages/:id/vote` |
| B4.11 | Tạo task iWork trực tiếp từ 1 tin nhắn ("1-click create task") | ✅ (giả lập, chỉ tạo tin nhắn bot) | `POST /api/tasks/create-from-message`; cần nối thật với dữ liệu task của MGR |
| B4.12 | Phòng họp video "Meet Now" (link Jitsi) + thẻ tin nhắn mời họp | ✅ | `POST /api/meetings` |
| B4.13 | Lịch sử/trạng thái cuộc họp (đang diễn ra, đã kết thúc) | 🟡 | lưu `status` nhưng không có API chuyển trạng thái kết thúc |
| B4.14 | Xem ảnh phóng to (lightbox) | ✅ | `modal-lightbox` |
| B4.15 | Bot hệ thống đăng tin tự động (nhận webhook từ MGR) | ✅ | `app.all(['/hooks/:hookId','/api/webhook'])` |
| B4.16 | Thread hội thoại dạng cây (reply-trong-reply nhiều cấp hiển thị lồng) | ⬜ | hiện reply chỉ trích dẫn 1 cấp |
| B4.17 | Mã hoá đầu-cuối / xoá tin tự động theo chính sách | ⬜ |

### B5. Tab theo kênh (Files / Tasks / About)

| # | Chức năng | Trạng thái |
|---|---|---|
| B5.1 | Tab "Tệp": lưới tệp/ảnh đã chia sẻ trong kênh, đăng tệp mới | ✅ | `GET /api/files` |
| B5.2 | Tab "iWork Tasks": danh sách task dự án đồng bộ từ MGR | 🟡 | UI có khung `channel-tasks-list`, chưa thấy API lấy task thật từ MGR (khác hệ thống dữ liệu) |
| B5.3 | Tab "Giới thiệu" (About): mô tả kênh, số thành viên, loại đồng bộ | ✅ | tĩnh theo kênh |
| B5.4 | Đồng bộ 2 chiều thật giữa Task trong MGR và tab "iWork Tasks" | ⬜ |

### B6. Hoạt động & Lịch họp & Tệp (Sidebar views)

| # | Chức năng | Trạng thái |
|---|---|---|
| B6.1 | Bản tin "Hoạt động" (nhắc tên, reaction, thông báo mới) | 🟡 | dữ liệu tĩnh trong HTML, cần nối API hoạt động thật |
| B6.2 | Sidebar "Lịch họp": bắt đầu họp ngay, lên lịch họp mới, danh sách cuộc họp | ✅ (bắt đầu ngay) / 🟡 (lên lịch cho tương lai — chưa có trường `scheduledAt` dùng tới trong UI đặt lịch) |
| B6.3 | Sidebar "Tệp chia sẻ": danh sách tệp toàn hệ thống | ✅ |
| B6.4 | Nhắc lịch họp qua email/notification trước giờ họp | ⬜ |

### B7. Quản lý nghỉ phép / báo bận (trong iChat)

> Lưu ý: đây là **hệ thống nghỉ phép thứ hai**, độc lập với "Bảng công & nghỉ phép" của MGR (`A9`) — cùng nghiệp vụ nhưng 2 nguồn dữ liệu khác nhau (`ichat/data/leaves.json` vs `S.st.leaves`).

| # | Chức năng | Trạng thái |
|---|---|---|
| B7.1 | Nộp đơn nghỉ phép/báo bận (loại: phép năm, ốm, báo bận, việc riêng) | ✅ | `leave-form`, `POST /api/leaves` |
| B7.2 | Bảng danh sách đơn & trạng thái, duyệt/từ chối (Leader/Admin) | ✅ | `PUT /api/leaves/:id` |
| B7.3 | Tự đăng thông báo đơn nghỉ mới vào kênh "Thông Báo Văn Phòng" | ✅ |
| B7.4 | **Hợp nhất 1 nguồn dữ liệu nghỉ phép duy nhất với MGR** (tránh 2 hệ thống rời) | ⬜ | cần quyết định kiến trúc khi làm bản thật |

### B8. Tích hợp liên-ứng dụng (MGR ↔ iChat)

| # | Chức năng | Trạng thái |
|---|---|---|
| B8.1 | MGR gửi thông báo sang iChat qua webhook khi có sự kiện (giao task, deadline, QC...) | ✅ (giả lập, `S.sendToIChat`) |
| B8.2 | iChat tạo task thật trong MGR (ghi vào store chung) khi dùng "1-click create task" | ⬜ | hiện chỉ đăng tin nhắn giả |
| B8.3 | Đăng nhập 1 lần (SSO) dùng chung giữa 2 ứng dụng | ⬜ |
| B8.4 | Nhúng MGR (iWork) làm tab trong Microsoft Teams thật | 🟡 | có hướng dẫn cấu hình trong Quản trị MGR (`A10.9`), cần kiểm thử thật trên Teams |

---

## C. Việc còn thiếu ở tầm hệ thống (không riêng 1 màn hình)

1. **Backend + cơ sở dữ liệu thật** cho cả 2 ứng dụng — hiện MGR dùng `localStorage` (chỉ 1 máy), iChat dùng file JSON (không khóa ghi đồng thời, không transaction).
2. **Một nguồn sự thật (single source of truth)** cho: người dùng, dự án/kênh, task, nghỉ phép — hiện bị trùng lặp giữa MGR và iChat.
3. **Phân quyền & xác thực thật ở server**, không chỉ chặn trên UI.
4. **API đồng bộ 2 chiều MGR ⇄ iChat** (tạo kênh khi tạo job, tạo task thật từ tin nhắn, đồng bộ trạng thái nghỉ phép).
5. **Thông báo thật** (email, push mobile) thay cho giả lập qua `localStorage`/webhook demo.
6. **Kiểm thử & CI**: hiện có Node script + Playwright chạy tay; cần pipeline CI tự động trước khi lên `iwork.i8studio.vn`.
7. **Khả năng mở rộng**: MGR render lại toàn trang bằng `innerHTML` mỗi lần đổi dữ liệu — cần đánh giá lại khi số lượng task/job tăng lớn (có thể cần SPA framework).
