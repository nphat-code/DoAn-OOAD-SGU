# BÀI THỰC HÀNH 3 (BTH3): SƠ ĐỒ CA SỬ DỤNG (USE CASE DIAGRAM)

**Môn học:** Phân tích thiết kế hướng đối tượng (OOAD) - SGU HK1 NH2025-2026  
**Tên đề tài:** Hệ thống Quản lý Cụm Sân Thể Thao và Đặt Sân Trực Tuyến  
*(Sports Complex Management & Online Court Booking System)*  

---

## PHẦN 1: XÁC ĐỊNH VÀ MÔ TẢ CÁC TÁC NHÂN (ACTORS)

Hệ thống bao gồm **5 tác nhân người dùng (Human Actors)** và **2 tác nhân hệ thống bên ngoài (External System Actors)**:

```mermaid
flowchart TD
    subgraph HumanActors ["👥 TÁC NHÂN NGƯỜI DÙNG"]
        Guest["👤 Khách hàng vãng lai<br>(Guest / Visitor)"]
        Customer["👤 Khách hàng thành viên<br>(Member Customer)"]
        Receptionist["👤 Nhân viên Lễ tân / Thu ngân<br>(Receptionist / Cashier)"]
        InventoryStaff["👤 Nhân viên Thủ kho<br>(Inventory Staff)"]
        Admin["👤 Quản trị viên / Chủ sân<br>(Manager / Administrator)"]
    end

    subgraph ExternalActors ["🌐 TÁC NHÂN HỆ THỐNG NGOÀI"]
        PaymentGW["💳 Cổng thanh toán trực tuyến<br>(VNPay / MoMo Gateway)"]
        NotifyGW["📱 Hệ thống SMS / Email Gateway<br>(Twilio / SendGrid / Firebase)"]
    end

    Customer -->|Kế thừa| Guest
```

### Bảng mô tả chi tiết vai trò của các Tác nhân:

| STT | Tác nhân (Actor) | Phân loại | Mô tả vai trò và trách nhiệm trong hệ thống |
| :---: | :--- | :---: | :--- |
| **1** | **Khách hàng vãng lai** *(Guest)* | Primary Actor | Người dùng chưa đăng nhập, chỉ có quyền tra cứu thông tin cụm sân, xem bảng giá theo giờ và kiểm tra lưới khung giờ còn trống (Timeline Grid). |
| **2** | **Khách hàng thành viên** *(Member Customer)* | Primary Actor *(Kế thừa Guest)* | Người chơi đã có tài khoản định danh, có thể đặt sân theo lượt, đăng ký lịch cố định theo tháng, thanh toán tiền cọc trực tuyến, nhận mã QR check-in, quản lý lịch sử đặt chỗ, hủy/đổi lịch và tích lũy điểm thưởng hội viên. |
| **3** | **Nhân viên Lễ tân / Thu ngân** *(Receptionist)* | Primary Actor | Người trực tiếp vận hành tại quầy: Quét mã QR check-in tiếp nhận khách, gán sân trực tiếp cho khách vãng lai tại chỗ, lập phiếu cho thuê dụng cụ thể thao (vợt/bóng), bán lẻ nước uống/phụ kiện, theo dõi cảnh báo lố giờ và lập hóa đơn thanh toán tổng hợp POS. |
| **4** | **Nhân viên Thủ kho** *(Inventory Staff)* | Primary Actor | Quản lý kho hàng hóa và tài sản: Tiếp nhận nhập kho nước uống/phụ kiện, theo dõi số lượng và tình trạng hư hỏng của dụng cụ cho thuê, lập phiếu kiểm kê và nhận cảnh báo khi tồn kho chạm mức tối thiểu. |
| **5** | **Quản trị viên / Chủ sân** *(Manager / Admin)* | Primary Actor | Người có quyền cao nhất: Thiết lập danh mục sân/loại sân/môn thể thao, cấu hình bảng giá ma trận (Giờ thường vs Giờ cao điểm, Ngày thường vs Cuối tuần/Lễ), cấu hình phụ thu & tỷ lệ hoàn cọc, quản lý nhân viên, phân quyền và theo dõi các báo cáo thống kê kinh doanh (Doanh thu, Tỷ lệ lấp đầy sân). |
| **6** | **Cổng thanh toán điện tử** *(Payment Gateway)* | Secondary Actor | Hệ thống bên thứ 3 (VNPay, MoMo, VietQR) chịu trách nhiệm xử lý các giao dịch thanh toán tiền cọc trực tuyến và trả về mã xác thực giao dịch (`Transaction_ID`) cho hệ thống. |
| **7** | **Hệ thống SMS/Email Gateway** | Secondary Actor | Dịch vụ gửi thông báo tự động (Mã OTP xác thực, Thông tin đặt sân thành công kèm mã QR check-in, Lời nhắc lịch chơi trước 2 tiếng). |

---

## PHẦN 2: SƠ ĐỒ USE CASE TỔNG THỂ HỆ THỐNG (SYSTEM OVERVIEW USE CASE DIAGRAM)

Sơ đồ tổng quan thể hiện sự phân bổ chức năng giữa 5 nhóm tác nhân chính và 5 phân hệ nghiệp vụ cốt lõi của hệ thống:

```mermaid
flowchart LR
    %% Actors
    ActorGuest["👤 Khách vãng lai"]
    ActorCustomer["👤 Khách thành viên"]
    ActorStaff["👤 Lễ tân / Thu ngân"]
    ActorWarehouse["👤 Nhân viên Thủ kho"]
    ActorAdmin["👤 Quản trị / Chủ sân"]

    %% Packages / Subsystems
    subgraph Sub1 ["1.0 PHÂN HỆ QUẢN LÝ DANH MỤC VÀ CẤU HÌNH"]
        UC_Config["Quản lý danh mục và cấu hình bảng giá"]
    end

    subgraph Sub2 ["2.0 PHÂN HỆ QUẢN LÝ ĐẶT SÂN VÀ LỊCH THI ĐẤU"]
        UC_Booking["Quản lý đặt sân và lịch thi đấu"]
    end

    subgraph Sub3 ["3.0 PHÂN HỆ QUẢN LÝ VẬN HÀNH TẠI SÂN"]
        UC_POS["Vận hành tại sân, dịch vụ phụ trợ & thanh toán POS"]
    end

    subgraph Sub4 ["4.0 PHÂN HỆ QUẢN LÝ KHO VÀ TÀI SẢN"]
        UC_Inventory["Quản lý kho và tài sản dụng cụ"]
    end

    subgraph Sub5 ["5.0 PHÂN HỆ QUẢN LÝ KHÁCH HÀNG VÀ BÁO CÁO"]
        UC_Report["Quản lý khách hàng, hội viên và báo cáo"]
    end

    %% Connections
    ActorGuest --> UC_Booking
    ActorCustomer --> UC_Booking
    ActorCustomer --> UC_Report

    ActorStaff --> UC_Booking
    ActorStaff --> UC_POS

    ActorWarehouse --> UC_Inventory

    ActorAdmin --> UC_Config
    ActorAdmin --> UC_Report
    ActorAdmin --> UC_Inventory
```

---

## PHẦN 3: SƠ ĐỒ USE CASE CHI TIẾT THEO TỪNG PHÂN HỆ

---

### 3.1. Phân hệ 1: Quản lý Danh mục và Cấu hình Bảng giá

Phân hệ này dành riêng cho **Quản trị viên / Chủ sân** nhằm thiết lập toàn bộ quy tắc vận hành và định giá cho cụm sân.

```mermaid
flowchart LR
    Admin["👤 Quản trị viên / Chủ sân"]

    subgraph Subsystem1 ["PHÂN HỆ 1: QUẢN LÝ DANH MỤC VÀ CẤU HÌNH"]
        UC11(["UC1.1: Quản lý danh mục sân"])
        UC111(["UC1.1.1: Thêm sân mới"])
        UC112(["UC1.1.2: Cập nhật thông tin sân"])
        UC113(["UC1.1.3: Chuyển trạng thái sân<br>(Hoạt động / Bảo trì / Tạm khóa)"])

        UC12(["UC1.2: Quản lý loại sân & Môn thể thao"])
        UC13(["UC1.3: Cấu hình bảng giá theo khung giờ"])
        UC131(["UC1.3.1: Thiết lập giá giờ tiêu chuẩn"])
        UC132(["UC1.3.2: Thiết lập giá giờ cao điểm (Peak-hours)"])

        UC14(["UC1.4: Cấu hình phụ thu và chính sách hoàn cọc"])
        UC141(["UC1.4.1: Cấu hình phụ thu cuối tuần / Ngày lễ"])
        UC142(["UC1.4.2: Cấu hình phụ thu quá giờ chơi"])
        UC143(["UC1.4.3: Cấu hình tỷ lệ hoàn cọc"])
    end

    Admin --- UC11
    Admin --- UC12
    Admin --- UC13
    Admin --- UC14

    UC111 -->|Kế thừa| UC11
    UC112 -->|Kế thừa| UC11
    UC113 -->|Kế thừa| UC11

    UC131 -->|Kế thừa| UC13
    UC132 -->|Kế thừa| UC13

    UC141 -->|Kế thừa| UC14
    UC142 -->|Kế thừa| UC14
    UC143 -->|Kế thừa| UC14
```

---

### 3.2. Phân hệ 2: Quản lý Đặt sân và Lịch thi đấu

Đây là phân hệ cốt lõi phục vụ **Khách hàng** đặt chỗ trực tuyến và **Lễ tân** xử lý giữ chỗ. Phân hệ tích hợp cơ chế chống trùng lịch (Lock slot 10 phút) và liên kết với Cổng thanh toán & SMS Gateway.

```mermaid
flowchart LR
    Guest["👤 Khách vãng lai"]
    Customer["👤 Khách thành viên"]
    PaymentGW["💳 Cổng thanh toán"]
    NotifyGW["📱 SMS/Email Gateway"]

    subgraph Subsystem2 ["PHÂN HỆ 2: QUẢN LÝ ĐẶT SÂN VÀ LỊCH THI ĐẤU"]
        UC21(["UC2.1: Tra cứu lịch sân trống (Timeline Grid)"])
        UC22(["UC2.2: Đặt sân theo lượt"])
        UC23(["UC2.3: Đặt lịch sân cố định theo tháng"])
        UC24(["UC2.4: Hủy đặt sân & Xử lý hoàn cọc"])
        UC25(["UC2.5: Điều chỉnh lịch đặt sân"])

        %% Supporting & Included Use Cases
        UC_Lock(["UC2.2.1: Khóa slot giữ chỗ tạm thời (10 phút)"])
        UC_Deposit(["UC2.2.2: Thanh toán tiền cọc trực tuyến"])
        UC_GenQR(["UC2.2.3: Phát hành mã đặt & Mã QR Check-in"])
        UC_CheckConflict(["UC2.3.1: Quét xung đột & Điều phối lịch tháng"])
    end

    Guest --- UC21
    Customer --- UC21
    Customer --- UC22
    Customer --- UC23
    Customer --- UC24
    Customer --- UC25

    %% Includes & Extends
    UC22 -.->|<<include>>| UC21
    UC22 -.->|<<include>>| UC_Lock
    UC22 -.->|<<include>>| UC_Deposit
    UC22 -.->|<<include>>| UC_GenQR

    UC23 -.->|<<include>>| UC_CheckConflict
    UC23 -.->|<<include>>| UC_Deposit

    UC_Deposit --- PaymentGW
    UC_GenQR --- NotifyGW
    UC24 --- NotifyGW
```

---

### 3.3. Phân hệ 3: Quản lý Vận hành tại sân

Phân hệ dành cho **Nhân viên Lễ tân / Thu ngân** thao tác trực tiếp tại quầy để đón khách, cung cấp dịch vụ phụ trợ và thu tiền.

```mermaid
flowchart LR
    Staff["👤 Nhân viên Lễ tân / Thu ngân"]

    subgraph Subsystem3 ["PHÂN HỆ 3: QUẢN LÝ VẬN HÀNH TẠI SÂN"]
        UC31(["UC3.1: Tiếp nhận & Check-in"])
        UC311(["UC3.1.1: Quét mã QR xác thực khách đặt trước"])
        UC312(["UC3.1.2: Mở sân trực tiếp cho khách vãng lai"])

        UC32(["UC3.2: Quản lý cho thuê dụng cụ"])
        UC321(["UC3.2.1: Lập phiếu mượn/thuê dụng cụ (Vợt, bóng)"])
        UC322(["UC3.2.2: Kiểm tra hoàn trả dụng cụ"])
        UC323(["UC3.2.3: Ghi nhận bồi thường hư hại dụng cụ"])

        UC33(["UC3.3: Bán lẻ nước giải khát & Phụ kiện"])
        UC34(["UC3.4: Theo dõi thời lượng & Cảnh báo quá giờ"])
        UC35(["UC3.5: Lập hóa đơn thanh toán tổng hợp"])

        %% Extends for Invoice
        UC_Overtime(["UC3.5.1: Tính phụ phí quá giờ chơi"])
        UC_Voucher(["UC3.5.2: Áp dụng Voucher / Giảm giá hội viên"])
    end

    Staff --- UC31
    Staff --- UC32
    Staff --- UC33
    Staff --- UC34
    Staff --- UC35

    UC311 -->|Kế thừa| UC31
    UC312 -->|Kế thừa| UC31

    UC32 -.->|<<include>>| UC321
    UC32 -.->|<<include>>| UC322
    UC323 -.->|<<extend>>| UC322

    UC_Overtime -.->|<<extend>>| UC35
    UC_Voucher -.->|<<extend>>| UC35

    UC32 -.->|<<include>>| UC35
    UC33 -.->|<<include>>| UC35
```

---

### 3.4. Phân hệ 4: Quản lý Kho và Tài sản

Phân hệ phục vụ **Nhân viên Thủ kho** và **Chủ sân** theo dõi số lượng tồn kho nước uống và tài sản vợt/bóng cho thuê.

```mermaid
flowchart LR
    WarehouseStaff["👤 Nhân viên Thủ kho"]
    Admin["👤 Quản trị viên / Chủ sân"]

    subgraph Subsystem4 ["PHÂN HỆ 4: QUẢN LÝ KHO VÀ TÀI SẢN"]
        UC41(["UC4.1: Quản lý danh mục hàng hóa & Dụng cụ"])
        UC42(["UC4.2: Lập phiếu nhập kho"])
        UC43(["UC4.3: Quản lý trang thiết bị / Dụng cụ cho thuê"])
        UC44(["UC4.4: Kiểm kê kho & Ghi nhận hao mòn"])
        UC45(["UC4.5: Cảnh báo tồn kho an toàn"])
    end

    WarehouseStaff --- UC41
    WarehouseStaff --- UC42
    WarehouseStaff --- UC43
    WarehouseStaff --- UC44
    WarehouseStaff --- UC45

    Admin --- UC41
    Admin --- UC44
    Admin --- UC45

    UC42 -.->|<<include>>| UC41
    UC44 -.->|<<extend>>| UC45
```

---

### 3.5. Phân hệ 5: Quản lý Khách hàng và Báo cáo

Phân hệ dành cho **Quản trị viên** theo dõi hiệu quả kinh doanh và quản lý người dùng, đồng thời cho phép **Khách hàng** quản lý thông tin hội viên.

```mermaid
flowchart LR
    Customer["👤 Khách thành viên"]
    Admin["👤 Quản trị viên / Chủ sân"]

    subgraph Subsystem5 ["PHÂN HỆ 5: QUẢN LÝ KHÁCH HÀNG VÀ BÁO CÁO"]
        UC51(["UC5.1: Quản lý khách hàng & Thẻ hội viên"])
        UC511(["UC5.1.1: Đăng ký / Nâng hạng thẻ hội viên"])
        UC512(["UC5.1.2: Tra cứu điểm tích lũy & Đổi ưu đãi"])

        UC52(["UC5.2: Quản lý chương trình khuyến mãi & Voucher"])

        UC53(["UC5.3: Báo cáo thống kê doanh thu"])
        UC531(["UC5.3.1: Báo cáo doanh thu tiền sân"])
        UC532(["UC5.3.2: Báo cáo doanh thu dịch vụ phụ trợ"])
        UC533(["UC5.3.3: Báo cáo doanh thu theo ca & Hình thức thanh toán"])

        UC54(["UC5.4: Báo cáo tỷ lệ lấp đầy sân (Occupancy Rate)"])
        UC55(["UC5.5: Quản trị tài khoản & Phân quyền nhân viên"])
    end

    Customer --- UC512

    Admin --- UC51
    Admin --- UC52
    Admin --- UC53
    Admin --- UC54
    Admin --- UC55

    UC511 -->|Kế thừa| UC51
    UC512 -->|Kế thừa| UC51

    UC531 -->|Kế thừa| UC53
    UC532 -->|Kế thừa| UC53
    UC533 -->|Kế thừa| UC53
```

---

## PHẦN 4: BẢNG PHÂN TÍCH QUAN HỆ GIỮA CÁC USE CASE

### 4.1. Bảng phân tích quan hệ `<<include>>` (Bắt buộc dùng chung)

| Base Use Case (UC gốc) | Included Use Case (UC được gọi) | Lý do nghiệp vụ bắt buộc |
| :--- | :--- | :--- |
| **UC2.2: Đặt sân theo lượt** | `UC2.1: Tra cứu lịch sân trống` | Khách hàng bắt buộc phải xem lịch trống mới có thể chọn sân và khung giờ phù hợp. |
| **UC2.2: Đặt sân theo lượt** | `UC2.2.1: Khóa slot giữ chỗ tạm thời` | Khi khách chọn sân, hệ thống lập tức khóa slot trong 10 phút để đảm bảo không ai khác đặt trùng. |
| **UC2.2: Đặt sân theo lượt** | `UC2.2.2: Thanh toán tiền cọc` | Đơn đặt sân chỉ có hiệu lực khi khách hoàn tất đặt cọc tối thiểu 30% qua cổng thanh toán. |
| **UC2.2: Đặt sân theo lượt** | `UC2.2.3: Phát hành mã đặt & QR` | Sau khi cọc thành công, hệ thống bắt buộc tạo mã định danh và mã QR để khách check-in tại sân. |
| **UC2.3: Đặt lịch theo tháng** | `UC2.3.1: Quét xung đột lịch` | Đặt lịch định kỳ bắt buộc phải quét toàn bộ các tuần trong tháng để phát hiện các ngày bị trùng lịch. |
| **UC3.5: Lập hóa đơn tổng hợp** | `UC3.2: Tiền thuê dụng cụ` *(nếu có)* | Hóa đơn thanh toán khi trả sân phải cộng dồn toàn bộ tiền thuê vợt/bóng chưa thanh toán. |
| **UC3.5: Lập hóa đơn tổng hợp** | `UC3.3: Tiền nước uống` *(nếu có)* | Hóa đơn thanh toán phải cộng dồn các mặt hàng nước uống/phụ kiện khách đã dùng trong ca chơi. |

---

### 4.2. Bảng phân tích quan hệ `<<extend>>` (Mở rộng có điều kiện)

| Base Use Case (UC gốc) | Extension Use Case (UC mở rộng) | Điểm mở rộng (Extension Point) | Điều kiện kích hoạt mở rộng |
| :--- | :--- | :--- | :--- |
| **UC3.5: Lập hóa đơn tổng hợp** | `UC3.5.1: Tính phụ phí quá giờ` | `At_Overtime_Calculation` | Khi thời gian khách trả sân vượt quá 15 phút so với giờ kết thúc đăng ký ban đầu. |
| **UC3.5: Lập hóa đơn tổng hợp** | `UC3.5.2: Áp dụng Voucher / Ưu đãi`| `At_Discount_Application` | Khi khách hàng xuất trình mã Voucher hợp lệ hoặc là Hội viên đạt hạng VIP/Gold. |
| **UC3.2.2: Kiểm tra trả dụng cụ**| `UC3.2.3: Ghi nhận đền bù hư hại` | `At_Equipment_Damage_Check` | Khi nhân viên phát hiện vợt bị gãy cán, nứt khung hoặc làm mất bóng thi đấu. |
| **UC4.4: Kiểm kê kho hàng** | `UC4.5: Cảnh báo tồn kho an toàn` | `At_Stock_Threshold_Check` | Khi số lượng nước uống hoặc phụ kiện trong kho giảm xuống dưới định mức an toàn quy định. |

---

### 4.3. Bảng phân tích quan hệ Kế thừa (Generalization)

* **Kế thừa giữa các Actor:**
  * `Khách hàng thành viên (Member Customer)` **kế thừa** `Khách hàng vãng lai (Guest)`: Kế thừa toàn bộ quyền xem lịch trống, xem giá và được bổ sung thêm quyền đặt chỗ, nạp cọc, tích điểm.
* **Kế thừa giữa các Use Case:**
  * `UC3.1.1: Quét QR check-in` và `UC3.1.2: Mở sân trực tiếp` là 2 dạng cụ thể hóa kế thừa từ `UC3.1: Tiếp nhận khách`.
  * `UC5.3.1: Báo cáo doanh thu tiền sân`, `UC5.3.2: Báo cáo doanh thu dịch vụ`, `UC5.3.3: Báo cáo theo ca` kế thừa từ `UC5.3: Báo cáo thống kê doanh thu`.

---

## PHẦN 5: MA TRẬN PHÂN QUYỀN ACTOR - USE CASE (ACCESS CONTROL MATRIX)

Bảng ma trận thể hiện quyền truy cập và thực thi của từng vai trò (Role) đối với toàn bộ 25 Use Case trong hệ thống:

| Mã UC | Tên Use Case | Khách vãng lai | Khách thành viên | Lễ tân / Thu ngân | Thủ kho | Quản trị / Chủ sân |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: |
| **UC1.1** | Quản lý danh mục sân (Thêm/Sửa/Đổi trạng thái) | | | | | **X** |
| **UC1.2** | Quản lý loại sân và môn thể thao | | | | | **X** |
| **UC1.3** | Cấu hình bảng giá ma trận theo khung giờ | | | | | **X** |
| **UC1.4** | Cấu hình phụ thu và chính sách hoàn cọc | | | | | **X** |
| **UC2.1** | Tra cứu lịch sân trống (Timeline Grid) | **X** | **X** | **X** | | **X** |
| **UC2.2** | Đặt sân theo lượt & Thanh toán cọc | | **X** | **X** | | **X** |
| **UC2.3** | Đặt lịch sân cố định theo tháng (Subscription) | | **X** | **X** | | **X** |
| **UC2.4** | Hủy đặt sân và xử lý hoàn cọc | | **X** | **X** | | **X** |
| **UC2.5** | Điều chỉnh / Dời lịch đặt sân | | **X** | **X** | | **X** |
| **UC3.1** | Tiếp nhận khách & Check-in (Quét QR / Mở sân) | | | **X** | | **X** |
| **UC3.2** | Cho thuê dụng cụ (Lập phiếu / Trả đồ / Đền bù) | | | **X** | | **X** |
| **UC3.3** | Bán lẻ nước giải khát & Phụ kiện tại quầy | | | **X** | | **X** |
| **UC3.4** | Theo dõi thời lượng & Cảnh báo quá giờ | | | **X** | | **X** |
| **UC3.5** | Lập hóa đơn thanh toán tổng hợp POS | | | **X** | | **X** |
| **UC4.1** | Quản lý danh mục hàng hóa & Dụng cụ | | | | **X** | **X** |
| **UC4.2** | Lập phiếu nhập kho | | | | **X** | **X** |
| **UC4.3** | Quản lý trang thiết bị & Dụng cụ cho thuê | | | | **X** | **X** |
| **UC4.4** | Kiểm kê kho & Ghi nhận hao mòn | | | | **X** | **X** |
| **UC4.5** | Cảnh báo tồn kho an toàn | | | | **X** | **X** |
| **UC5.1** | Quản lý khách hàng & Thẻ hội viên | | **X** *(Xem điểm)* | **X** *(Tra cứu)* | | **X** *(Toàn quyền)* |
| **UC5.2** | Quản lý chương trình khuyến mãi & Voucher | | **X** *(Xem)* | **X** *(Áp dụng)* | | **X** *(Tạo mới)* |
| **UC5.3** | Báo cáo thống kê doanh thu (Sân / Dịch vụ / Ca) | | | **X** *(Theo ca)* | | **X** *(Toàn bộ)* |
| **UC5.4** | Báo cáo tỷ lệ lấp đầy sân (Occupancy Rate) | | | | | **X** |
| **UC5.5** | Quản trị tài khoản & Phân quyền người dùng | | | | | **X** |

---

## PHẦN 6: ĐỐI CHIẾU TIÊU CHÍ CHẤM ĐIỂM OOAD SGU (BTH3)

1. **Tính đầy đủ và bao phủ (Completeness):** Sơ đồ Use Case bao phủ 100% các chức năng đã phân rã trong BFD (BTH1), không bỏ sót bất kỳ quy trình nào từ khảo sát BTH2.
2. **Tính chuẩn xác của ký pháp UML (Syntactic Correctness):**
   * Sử dụng đúng quan hệ `<<include>>` (mũi tên đứt nét trỏ từ UC chính sang UC bắt buộc).
   * Sử dụng đúng quan hệ `<<extend>>` (mũi tên đứt nét trỏ từ UC mở rộng về UC chính kèm Extension Point).
   * Phân biệt rõ ràng Actor chính (Human) và Actor phụ (Hệ thống cổng thanh toán / SMS).
3. **Tính sẵn sàng cho BTH4 (Đặc tả Use Case):** Danh sách 25 Use Case này là đầu vào trực tiếp để lựa chọn và viết đặc tả chi tiết trong BTH4.
