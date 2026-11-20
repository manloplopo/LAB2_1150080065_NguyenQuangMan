# LAB 2: MÃ HÓA ĐỐI XỨNG HIỆN ĐẠI
**Học phần:** An toàn và Bảo mật thông tin

## Thông tin sinh viên
* **Họ và tên:**Nguyễn Quang Mẫn
* **MSSV:** 1150080065
* **Lớp:** 11_THMT


---

## 1. Yêu cầu môi trường & Phần mềm
Để chạy đầy đủ các bài thực hành (từ 2.1 đến 2.6), máy tính cần cài đặt sẵn:
* **Java Development Kit (JDK):** Phiên bản 17 hoặc 21.
* **IDE:** Apache NetBeans (phiên bản 20 trở lên) có kết nối Internet để Maven tải các thư viện phụ thuộc (`mssql-jdbc` và `bcprov-jdk18on`).
* **Cơ sở dữ liệu:** Microsoft SQL Server Express + SQL Server Management Studio (SSMS).
* **Python:** Phiên bản 3.10 trở lên (kèm thư viện `cryptography`) cho bài nâng cao 02.

---

## 2. Cấu hình Cơ sở dữ liệu (SQL Server)
Trước khi chạy phần đăng ký/đăng nhập (Bài 2.3) và lưu lịch sử thao tác, cần thiết lập SQL Server theo các bước sau:

1. **Cấu hình mạng & Bảo mật:**
   * Mở **SQL Server Configuration Manager** $\rightarrow$ **Protocols for SQLEXPRESS** $\rightarrow$ bật **TCP/IP (Enabled)**. Tại mục **IPAll**, đặt `TCP Port` là `14333` và để trống `TCP Dynamic Ports`. Khởi động lại dịch vụ SQL Server.
   * Mở **SSMS**, nhấp phải vào Server $\rightarrow$ **Properties** $\rightarrow$ **Security** $\rightarrow$ chọn chế độ **SQL Server and Windows Authentication mode**.
2. **Tạo CSDL và Tài khoản:**
   * Mở và chạy toàn bộ script `01_Tao_CSDL.sql` để tạo database `LAB2_MaHoaHienDai` cùng 2 bảng `dbo.app_users` và `dbo.crypto_history`.
   * Mở và chạy toàn bộ script `02_Tao_Tai_Khoan_SQL.sql` để tạo tài khoản đăng nhập mặc định:
     * **User:** `lab2_user`
     * **Password:** `LabUser@2026`

---

## 3. Hướng dẫn chạy chương trình

### Phần A: Chương trình chính bằng Java (Bài 2.1 – 2.6)
1. Mở **Apache NetBeans**, chọn **File** $\rightarrow$ **Open Project...** và trỏ tới thư mục project `LAB2_MaHoaHienDai` (nằm trong `C:\BaoMat`).
2. Nhấp phải vào tên project chọn **Clean and Build** (hoặc nhấn `Shift + F11`) để Maven tải thư viện và biên dịch mã nguồn. Đảm bảo xuất hiện thông báo `BUILD SUCCESS`.
3. Nhấp phải vào tên project chọn **Run** (hoặc nhấn `F6`) để khởi chạy giao diện menu chính (`week_02.frm_Menu`).
4. **Kết nối SQL Server trên ứng dụng:**
   * Trên cửa sổ `frm_Menu`, bấm nút **Kết nối SQL Server**.
   * Nhập thông tin: Server `127.0.0.1`, Port `14333`, Database `LAB2_MaHoaHienDai`, User `lab2_user`, Password `LabUser@2026`.
   * Bấm **Test Connection** để xác nhận kết nối thành công trước khi sử dụng chức năng đăng nhập hoặc ghi lịch sử.
5. Chọn các nút chức năng tương ứng trên Menu để thực hành:
   * **2.1 DES:** Mã hóa/giải mã DES-CBC (Khóa 8 ký tự không dấu, ví dụ: `12345678`).
   * **2.2 TripleDES:** Mã hóa/giải mã 3DES-CBC (Khóa 24 ký tự không dấu, ví dụ: `123456789012345678901234`).
   * **2.3 AES + Login:** Đăng ký, đăng nhập tài khoản lưu trên SQL Server, nhận Ticket phiên làm việc và thực hiện mã hóa/giải mã AES-256-GCM.
   * **2.4 RC4:** Mã hóa/giải mã dòng RC4, xuất kết quả dạng HEX.
   * **2.5 Twofish:** Mã hóa/giải mã Twofish-CBC (sinh IV ngẫu nhiên, hỗ trợ lưu/mở tệp kèm IV).
   * **2.6 Nâng cao - AES-CTR:** Mã hóa/giải mã AES-256 chế độ CTR (sinh Key và Nonce ngẫu nhiên).
   * **Xem lịch sử:** Theo dõi nhật ký các thao tác mã hóa/giải mã từ bảng `dbo.crypto_history`.

### Phần B: Bài nâng cao 02 – AES-256-CBC bằng Python
1. Mở **Command Prompt (CMD)** và cài đặt thư viện mã hóa (chỉ cần thực hiện 1 lần):
   ```bash
   py -m pip install cryptography
   ```
2. Di chuyển vào thư mục chứa mã nguồn Python:
   ```bash
   cd /d C:\BaoMat\LAB2_Python
   ```
3. **Chạy kiểm thử tự động (Self-test):**
   ```bash
   py aes_cbc_gui.py --self-test
   ```
   *(Kết quả hiển thị `SELF-TEST OK` là đạt yêu cầu).*
4. **Chạy ứng dụng giao diện (Tkinter):**
   ```bash
   py aes_cbc_gui.py
   ```
   * Trên giao diện, bấm **Generate Key** và **Generate IV**, nhập nội dung vào ô **Plaintext** và bấm **Encrypt** / **Decrypt** để kiểm tra.