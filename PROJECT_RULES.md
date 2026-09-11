# QUY TẮC PHÁT TRIỂN & TIÊU CHUẨN ĐỒ ÁN OOAD (SGU)

Tài liệu này ghi nhớ toàn bộ các quy tắc, ràng buộc và tiêu chuẩn vận hành mà AI Assistant và nhóm thực hiện tuân thủ trong suốt quá trình phát triển đồ án môn học **Phân tích thiết kế hướng đối tượng (OOAD) - Trường Đại học Sài Gòn (SGU) HK1 NH2025-2026**.

---

## 1. QUY TẮC LƯU TRỮ VÀ ĐỊNH DẠNG TÀI LIỆU
* **Lưu trữ tức thì (Instant Persistence)**: Làm xong phần nào (bài thực hành, sơ đồ, đặc tả, mã nguồn) phải lập tức ghi lưu vào file trong thư mục dự án `DoAn-OOAD/`, không chỉ trả lời dạng tin nhắn trong chat.
* **Định dạng tài liệu**:
  * **CHỈ sử dụng định dạng Markdown (`.md`)** và văn bản/mã nguồn thuần túy.
  * **TUYỆT ĐỐI KHÔNG tự động tạo file Word `.docx`** (trừ khi người dùng yêu cầu xuất file nộp cụ thể).
* **Trực quan hóa sơ đồ**: Mọi sơ đồ phân tích nghiệp vụ (BFD, Use Case, Sequence, Class Diagram, State Machine, Gantt) phải được viết bằng cú pháp **Mermaid diagram** chuẩn bên trong file Markdown để có thể render trực quan ngay lập tức.
* **Cấu hình đọc file**: Luôn duy trì cấu hình trong `.vscode/settings.json` để IDE tự động mở các file `.md` ở chế độ **Rich Preview**.

---

## 2. QUY TẮC QUẢN LÝ PHIÊN BẢN (GIT & GITHUB)
* **Vị trí Git Repository**: Nằm trực tiếp bên trong thư mục đồ án:  
  `c:\Study\HK1Nam3\PTTKHDT\ChuongTrinh\DoAn-OOAD\`
* **Nhánh làm việc chính**: `main`
* **Remote Repository**: `https://github.com/nphat-code/DoAn-OOAD-SGU.git`
* **Quy chuẩn hiển thị tiếng Việt**: `core.quotepath = false` (không bị lỗi mã hóa ký tự Unicode).
* **Quy trình Commit & Push**:
  * Sau khi hoàn thành hoặc cập nhật một hạng mục công việc (bài thực hành BTHx, tính năng, sơ đồ mới), tự động tạo commit có ý nghĩa theo chuẩn Conventional Commits (ví dụ: `feat(bth3): ...`, `docs(rules): ...`).
  * Tự động push lên GitHub để đồng bộ tiến độ đám mây.

---

## 3. CẤU TRÚC THƯ MỤC CHUẨN ĐỒ ÁN (THEO YÊU CẦU SGU)
```text
DoAn-OOAD/
├── readme.txt                             # Thông tin nhóm, đề tài, phân công
├── PROJECT_RULES.md                      # Bộ quy tắc vận hành & tiêu chuẩn đồ án
├── BaoCao/                                # Báo cáo tiến độ và báo cáo tổng hợp
├── BTH/                                   # Tài liệu chi tiết 8 bài thực hành
│   ├── BTH1_MoTaDeTai_BFD/                # [DONE] Mô tả nghiệp vụ & Sơ đồ BFD
│   ├── BTH2_KeHoach_KhaoSat/              # [DONE] Kế hoạch 12 tuần & Khảo sát Jotform
│   ├── BTH3_UsecaseDiagram/               # Sơ đồ Use Case tổng thể & từng phân hệ
│   ├── BTH4_DacTaUsecase/                 # Đặc tả chi tiết từng Use Case
│   ├── BTH5_SequenceDiagram/              # Sơ đồ tuần tự các ca sử dụng chính
│   ├── BTH6_ClassDiagram/                 # Sơ đồ lớp đối tượng & bảng mô tả
│   ├── BTH7_ThietKeCSDL/                  # Thiết kế CSDL quan hệ RDM chuẩn 3NF
│   └── BTH8_ThietKeGiaoDien/              # Mockup UI & Bảng mô tả biến cố sự kiện
└── ChuongTrinh/
    ├── Source code/                       # Toàn bộ mã nguồn ứng dụng chạy được
    ├── Database/                          # File script cài đặt CSDL (.sql)
    ├── Poster/                            # File ảnh poster A4 giới thiệu đồ án
    └── Tham Khao/                         # Tài liệu tham khảo, chuẩn thiết kế
```

---

## 4. TIÊU CHUẨN NGHIỆP VỤ & HƯỚNG ĐỐI TƯỢNG (OOAD)
* **Tên đề tài chính thức**: *Hệ thống Quản lý Cụm Sân Thể Thao và Đặt Sân Trực Tuyến (Sports Complex & Court Booking Management System)*.
* **Mô hình hóa Hướng đối tượng thực thụ**:
  * **Kế thừa (Inheritance)**: Phân cấp loại sân (`Court` -> `BadmintonCourt`, `PickleballCourt`), loại đặt chỗ (`SingleBooking`, `RecurringBooking`), sản phẩm kho (`RentalEquipment`, `ConsumableItem`).
  * **Đa hình (Polymorphism)**: Phương thức tính giá linh hoạt theo loại sân, khung giờ cao điểm (Peak-hours 17h-22h) và ngày cuối tuần/ngày lễ.
  * **Chứa đựng & Hóa đơn tổng hợp (Composite Pattern)**: Một `Invoice` tích hợp tiền sân + phụ phí quá giờ + tiền thuê dụng cụ (vợt/bóng) + tiền nước giải khát - tiền cọc - ưu đãi hội viên.
  * **Quản lý vòng đời trạng thái (State Machine)**:
    * Sân: `Available` $\rightarrow$ `Booked` $\rightarrow$ `Occupied` $\rightarrow$ `Maintenance`.
    * Dụng cụ thuê: `InStock` $\rightarrow$ `Rented` $\rightarrow$ `Returned` (hoặc `Damaged/Penalty`).
* **Chiến lược Vấn đáp & Live Coding**:
  * Mã nguồn phân tầng rõ ràng (Presentation -> Business/Service -> Data Access / Repository).
  * Đảm bảo tính mở rộng cao để khi Thầy/Cô yêu cầu sửa trực tiếp chức năng trong phòng thi (thêm loại sân, đổi chính sách phụ phí, bổ sung thuộc tính) thì có thể sửa và demo chạy ngay trong 2-3 phút.

---

## 5. NGUYÊN TẮC THỐNG NHẤT XUYÊN SUỐT (TRACEABILITY MATRIX TỪ BTH1 - BTH8)
Tuyệt đối không để xảy ra tình trạng "bài này một chức năng, bài kia lại chức năng khác". Toàn bộ 8 bài thực hành phải tuân thủ nghiêm ngặt ma trận ánh xạ 1 - 1:

1. **Bộ Actor cố định**:
   * Khách vãng lai (`Guest`), Khách thành viên (`Member Customer`), Lễ tân/Thu ngân (`Receptionist`), Thủ kho (`Inventory Staff`), Quản trị/Chủ sân (`Manager/Admin`), Cổng thanh toán (`Payment Gateway`), Dịch vụ SMS/Email.
2. **Vòng đời trạng thái sân (Court Lifecycle)**:
   * `Available` $\rightarrow$ `Booked` (Khóa tạm 10p / Đã cọc 30%) $\rightarrow$ `Occupied` (Check-in QR) $\rightarrow$ `Maintenance`.
3. **Công thức Hóa đơn tổng hợp (Composite Invoice)**:
   $$\text{Tổng tiền} = \text{Tiền sân} + \text{Phụ phí quá giờ} + \text{Tiền thuê dụng cụ} + \text{Tiền nước/phụ kiện} - \text{Tiền cọc} - \text{Voucher}$$
4. **Ánh xạ 1 - 1 giữa các bài thực hành**:
   * **BTH1 (BFD 5 phân hệ)** $\rightarrow$ **BTH3 (Use Case)** $\rightarrow$ **BTH4 (Đặc tả)** $\rightarrow$ **BTH5 (Sequence)** $\rightarrow$ **BTH6 (Class Diagram)** $\rightarrow$ **BTH7 (CSDL 3NF)** $\rightarrow$ **BTH8 (Giao diện UI & Events)** $\rightarrow$ **Source Code**. Mọi danh từ, động từ, thực thể, nghiệp vụ phải đồng bộ 100%.

