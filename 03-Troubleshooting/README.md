![EVE-NG Banner](https://github.com/user-attachments/assets/139141be-596a-417c-9eb6-885af59cd78c)

# HƯỚNG DẪN XỬ LÝ LỖI TRÊN EVE-NG/PNETLAB

Tài liệu này tổng hợp các lỗi thường gặp khi sử dụng EVE-NG/PNETLAB và cách khắc phục chi tiết. Mỗi lỗi được mô tả rõ ràng về hiện tượng và các bước xử lý cụ thể.

## 📋 Mục Lục
- [Lỗi "Fix Permission"](#1-lỗi-fix-permission)
- [Lỗi Image QEMU Tự Động Stop](#2-lỗi-image-qemu-tự-động-stop)
- [Lỗi "Virtualized Intel VT-X/EPT Not Supported"](#3-lỗi-virtualized-intel-vt-xept-not-supported)
- [Lỗi Connection Abandoned Khi Dùng Wireshark](#4-lỗi-connection-abandoned-khi-dùng-wireshark)
- [Hỗ Trợ](#-hỗ-trợ)

---

## 1. LỖI "FIX PERMISSION"

### 🔴 Hiện Tượng

Khi thực hiện lệnh fix permissions sau khi thêm image:

```bash
/opt/unetlab/wrapper/unl_wrapper -a fixpermissions
```

Hệ thống báo lỗi:

```
PHP Warning: file_get_contents (/opt/unetlab/platform/): failed to open stream:
No such file or directory in /opt/unetlab/html/includes/init.php on line 71
```

### ✅ Cách Khắc Phục

#### Bước 1: Kiểm Tra Platform CPU

Chạy lệnh để xác định loại CPU:

```bash
dmesg | grep -i cpu | grep -i -e intel -e amd
```

#### Bước 2: Tạo File Platform

**Nếu kết quả hiển thị "Intel":**

```bash
echo "intel" > /opt/unetlab/platform
```

**Nếu kết quả hiển thị "AMD":**

```bash
echo "amd" > /opt/unetlab/platform
```

#### Bước 3: Chạy Lại Fix Permissions

```bash
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

✅ **Hoàn tất!** Lỗi đã được khắc phục.

---

## 2. LỖI IMAGE QEMU TỰ ĐỘNG STOP

### 🔴 Hiện Tượng

Sau khi thêm các image QEMU (Fortigate, Windows, v.v.) và start thiết bị:
- Thiết bị khởi động
- Sau đó **tự động stop** ngay lập tức
- Không thể duy trì trạng thái running

### ✅ Cách Khắc Phục

Bật tính năng ảo hóa phần cứng trong cài đặt CPU của máy ảo EVE-NG:

#### Bước 1: Tắt Máy Ảo EVE-NG

Tắt hoàn toàn máy ảo EVE-NG (không phải Suspend).

#### Bước 2: Vào Settings CPU

1. Click phải vào máy ảo EVE-NG → **Settings**
2. Chọn tab **Hardware** → **Processors**
3. Tìm mục **Virtualization engine**

#### Bước 3: Bật Nested Virtualization

Tick vào ô:
- ✅ **Virtualize Intel VT-x/EPT** (cho CPU Intel)
- ✅ **Virtualize AMD-V/RVI** (cho CPU AMD)

![image.png](https://github.com/user-attachments/assets/bd3a41a1-d00b-44f2-934c-e3d255a8e6aa)

![image.png](https://github.com/user-attachments/assets/ca9843fc-ef9a-4b17-8e13-cfde260f9edb)

#### Bước 4: Khởi Động Lại Máy Ảo

Click **OK** và khởi động lại máy ảo EVE-NG.

✅ **Hoàn tất!** Image QEMU giờ đây sẽ chạy bình thường.

---

## 3. LỖI "VIRTUALIZED INTEL VT-X/EPT NOT SUPPORTED"

### 🔴 Hiện Tượng

Khi bật **Virtualize Intel VT-x/EPT** hoặc **AMD-V/RVI** trong cài đặt CPU của máy ảo EVE-NG, máy ảo không thể khởi động và hiển thị lỗi:

```
Virtualized Intel VT-X/EPT is not supported on this platform.
Continue without virtualization engine?
```

![image.png](https://github.com/user-attachments/assets/a1539f2a-2483-45fc-975f-6c1aaee4bd25)

### ✅ Cách Khắc Phục

Lỗi này xảy ra do **Hyper-V** của Windows đang xung đột với VMware. Cần tắt Hyper-V hoàn toàn.

#### 🔧 Phương Án 1: Tự Động (Khuyến Nghị)

**Cách 1:** Sử dụng tool của BlueStacks

1. Tải file: [HD-DisableHyperV_native_v2.exe](https://cdn3.bluestacks.com/support_files/HD-DisableHyperV_native_v2.exe)
2. Chạy file với quyền **Administrator**
3. Khởi động lại máy tính

**Cách 2:** Nếu tool trên không hoạt động

1. Tải file: [DisableVBS.rar](DisableVBS.rar)
2. Giải nén và chạy với quyền **Administrator**
3. Khởi động lại máy
4. Nhấn **F3** vài lần khi có câu hỏi

💡 **Lưu ý:** Các tool trên sẽ tự động thực hiện các bước trong "Phương án 2".

#### ⚙️ Phương Án 2: Thủ Công

##### Bước 1: Tắt Hyper-V trong Windows Features

1. Mở **Control Panel** → **Programs** → **Turn Windows features on or off**
2. Bỏ tick ✅ **Hyper-V**
3. Click **OK** và khởi động lại nếu được yêu cầu

##### Bước 2: Tắt Hyper-V qua CMD

1. Mở **Command Prompt** với quyền **Administrator**
2. Chạy lệnh:

```bash
bcdedit /set hypervisorlaunchtype off
```

##### Bước 3: Tắt Hyper-V qua PowerShell

1. Mở **PowerShell** với quyền **Administrator**
2. Chạy lệnh:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All
```

##### Bước 4: Khởi Động Lại Máy

```bash
shutdown /r /t 0
```

✅ **Hoàn tất!** Bây giờ bạn có thể bật Nested Virtualization trong VMware.

---

## 4. LỖI CONNECTION ABANDONED KHI DÙNG WIRESHARK

### 🔴 Hiện Tượng

Khi capture traffic từ EVE-NG:

1. Click phải vào thiết bị → **Capture**
2. Chọn interface cần capture
3. Cửa sổ CMD hiện lên với thông báo: **"Connection abandoned"**
4. Wireshark vẫn mở nhưng **không bắt được gói tin nào**

### ✅ Cách Khắc Phục

#### Bước 1: Mở Command Prompt tại Thư Mục EVE-NG

1. Mở thư mục cài đặt EVE-NG Client (mặc định: `C:\Program Files\EVE-NG`)
2. Trong File Explorer, gõ `cmd` vào thanh địa chỉ và nhấn **Enter**

#### Bước 2: Thiết Lập SSH Connection

Chạy lệnh sau (thay `<IP-của-EVE>` bằng địa chỉ IP thực tế):

```bash
plink root@<IP-của-EVE>
```

**Ví dụ:**

```bash
plink root@192.168.65.134
```

#### Bước 3: Xác Nhận Kết Nối

1. Khi hỏi **"Store key in cache? (y/n)"**, gõ `y` và nhấn **Enter**
2. Nhập **password root** của EVE-NG (mật khẩu mà bạn đã đặt khi cài đặt)

✅ **Hoàn tất!** Bây giờ Wireshark sẽ capture traffic bình thường.

💡 **Lưu ý:** Chỉ cần thực hiện thao tác này **một lần duy nhất**. Lần sau capture sẽ hoạt động tự động.

---

## 📞 Hỗ Trợ

### Các vấn đề thường gặp khác:

**💬 Cộng đồng:**
- [EVE-NG Community Forum](https://www.eve-ng.net/index.php/community/)
- [EVE-NG Reddit](https://www.reddit.com/r/eve/)

**📚 Tài liệu tham khảo:**
- [EVE-NG Official Documentation](https://www.eve-ng.net/index.php/documentation/)
- [EVE-NG Cookbook](https://www.eve-ng.net/index.php/documentation/eve-ng-cookbook/)
- [VMware Nested Virtualization Guide](https://docs.vmware.com/en/VMware-Workstation-Pro/)

**🐛 Báo lỗi:**
- Tạo issue trên repository này với thông tin chi tiết:
  - Phiên bản EVE-NG
  - Thông báo lỗi cụ thể
  - Screenshot (nếu có)
  - Các bước đã thử

---

## 💡 Lưu Ý Quan Trọng

### 🔐 Bảo Mật
- Không chia sẻ password root của EVE-NG
- Sử dụng password mạnh
- Thay đổi password mặc định ngay sau khi cài đặt

### ⚙️ Hiệu Năng
- Cấp đủ RAM và CPU cho máy ảo EVE-NG
- Bật VT-x/AMD-V trong BIOS của máy host
- Đảm bảo có đủ dung lượng ổ cứng trống

### 💾 Backup
- Backup cấu hình EVE-NG định kỳ
- Export các lab quan trọng
- Lưu trữ image ở vị trí an toàn

---

## 📄 License

Hướng dẫn này được tạo ra cho mục đích chia sẻ kiến thức và hỗ trợ cộng đồng.

---

## 📚 Tài Nguyên Liên Quan

- [Hướng Dẫn Cài Đặt EVE-NG](../01-Installation/README.md)
- [Hướng Dẫn Cài Đặt ishare2](../02-iShare2-Images/README.md)
- [EVE-NG Official Website](https://www.eve-ng.net/)

---

**⭐ Nếu hướng dẫn này hữu ích, đừng quên star repo này!**

---

<div align="center">

[↑ Về đầu trang](#hướng-dẫn-xử-lý-lỗi-trên-eve-ngpnetlab)

</div>