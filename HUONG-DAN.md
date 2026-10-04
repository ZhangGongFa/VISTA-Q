# Gói báo cáo kết quả VISTA-Q

- Mở `VISTA-Q-Bao-cao-ket-qua.html` bằng Chrome hoặc Edge. HTML chứa số liệu và chạy offline; không cần Colab, Python, server hoặc Internet.
- Đọc `Bao-cao-danh-gia-tong-the-VISTA-Q.pdf` để trình Thầy. Bản Markdown cùng tên giữ nội dung có thể chỉnh sửa.
- Giữ các file trong cùng thư mục để link PDF trong HTML hoạt động. Có thể gửi riêng HTML; tất cả số liệu và biểu đồ vẫn hoạt động.
- Bộ lọc ở phần dự báo điều khiển forecast, interval, queue và horizon portfolio. Portfolio descriptive metrics và NAV dùng toàn giai đoạn; paired inference dùng period đang chọn. Tra cứu bảng có bộ lọc riêng và mặc định giữ toàn bộ dòng.
- “In báo cáo” in tóm tắt cùng hình/bảng đang chọn. “In toàn văn” thêm mọi chương báo cáo. Không dùng bản in để thay thế CSV khi kiểm tra số gốc.
- `tables/` giữ 13 CSV nguồn, không sửa trị số; `evidence_data.json` giữ bảng, snapshot, 64 curves, nội dung báo cáo và SHA-256 nguồn.
- Đây là bản tổng hợp hồi cứu, không phải feed trực tiếp; không có dữ liệu analyst review riêng, thông tin đăng nhập hoặc chức năng đặt lệnh.

Ngày tổng hợp: 04/10/2026. Snapshot: 21/09/2026. V92 audit 4ea42ef8f6c5022b; integration e751fc0f0c7991fd.

PDF được dàn trang trực tiếp từ cùng nội dung và số liệu với HTML. Bộ render Word đóng gói không có LibreOffice và Microsoft Word automation không khởi tạo được trên máy này; không phát hành bản DOCX chưa được kiểm tra pagination native.
