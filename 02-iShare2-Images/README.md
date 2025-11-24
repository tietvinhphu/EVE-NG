![EVE-NG Banner](https://github.com/user-attachments/assets/139141be-596a-417c-9eb6-885af59cd78c)

# HƯỚNG DẪN CÀI ĐẶT ISHARE2 CHO EVE-NG/PNETLAB

**ishare2** là công cụ CLI mạnh mẽ giúp tìm kiếm và tải image cho các thiết bị mạng trong EVE-NG/PNETLAB một cách nhanh chóng và dễ dàng. Thay vì phải tìm kiếm và upload image thủ công, bạn chỉ cần gõ lệnh để tải về từ kho lưu trữ tập trung.

📌 **Github Repository:** [ishare2-org/ishare2-cli](https://github.com/ishare2-org/ishare2-cli)

## 📋 Mục Lục
- [Chuẩn Bị Hệ Thống](#1-chuẩn-bị-hệ-thống-prerequisites)
- [Cài Đặt ishare2](#2-cài-đặt-ishare2-phương-pháp-github)
- [Quy Trình Tải Image](#3-quy-trình-tải-image-chuẩn-sop)
- [Xử Lý License Cisco IOL](#4-xử-lý-lỗi-license-cisco-iol-python-3-fix)
- [Danh Sách Image Khuyến Nghị](#5-danh-sách-image-khuyến-nghị-best-practices)
- [Quản Lý Và Tối Ưu Hóa](#6-quản-lý-và-tối-ưu-hóa-tài-nguyên)
- [Hỗ Trợ](#-hỗ-trợ)

---

## 1. CHUẨN BỊ HỆ THỐNG (PREREQUISITES)

Trước khi cài đặt, hệ thống cần đảm bảo kết nối Internet và khả năng phân giải tên miền (DNS).

### 1.1. Cấu hình DNS

Mặc định EVE-NG có thể nhận DNS không ổn định. Cần gán cứng DNS Google:

```bash
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

### 1.2. Kiểm tra kết nối

Thực hiện Ping để xác nhận mạng thông suốt:

```bash
ping -c 4 google.com
```

- **Đạt:** Kết quả trả về `64 bytes from...`
- **Không đạt:** Kiểm tra Card mạng VMware (Bắt buộc chế độ **NAT**).

---

## 2. CÀI ĐẶT ISHARE2 (PHƯƠNG PHÁP GITHUB)

### 2.1. Kết Nối SSH

SSH vào giao diện CLI của EVE (hoặc dùng luôn giao diện CLI trên máy ảo, nhưng SSH dễ làm hơn)

![image.png](https://github.com/user-attachments/assets/e8a04ac2-d869-4b95-a2c4-a472e027631a)

⚠️ **Lưu ý:** Do server trang chủ thường xuyên gián đoạn, sử dụng mã nguồn trực tiếp từ GitHub để đảm bảo tính ổn định.

### 2.2. Chạy Lệnh Cài Đặt

```bash
wget -O /usr/sbin/ishare2 https://raw.githubusercontent.com/ishare2-org/ishare2-cli/main/ishare2 && chmod +x /usr/sbin/ishare2
```

![image.png](https://github.com/user-attachments/assets/f453c973-3d4c-4477-bbc4-b541a695e077)

Đoạn script sẽ tự động cài các package cần thiết, sau đó cài **ishare2** vào EVE.

![image.png](https://github.com/user-attachments/assets/9c8e79cf-c660-48d6-a234-645869114f1d)

### 2.3. Cấu Hình ishare2

Trong quá trình cài đặt, hệ thống sẽ hỏi các câu hỏi cấu hình:

```bash
[+] Use aria2c for faster downloads? (default: no)
[+] (y/n): **y**
```

Nếu nó đứng chỗ này thì bấm **ENTER**

![image.png](https://github.com/user-attachments/assets/b88c5a9c-d516-4fdb-8460-637d8a7bb174)

```bash
[+] Check SSL certificate? (default: yes)
[+] (y/n): **n**
```

```bash
[+] Choose the update channel.
 1) dev
 2) main
[*] Enter the number of the branch you want to use (default: main): **enter**
```

```bash
[+] Choose a mirror. (default: Rotate mirrors)
 1) Rotate mirrors (recommended)
 2) Google Drive mirror
 3) Onedrive mirror
 4) Custom mirror
[*] Enter the number of the mirror you want to use (default: 1): **enter**
```

### 2.4. Kiểm Tra Cài Đặt

Gõ lệnh `ishare2` để kiểm tra:

```bash
ishare2
```

✅ **Thành công:** Nếu hiển thị menu Help như hình dưới đây là cài đặt thành công.

![image.png](https://github.com/user-attachments/assets/1e2f0e16-bcbc-42f7-8e47-b327e2c5b730)

---

## 3. QUY TRÌNH TẢI IMAGE CHUẨN (SOP)

Quy trình tải image chuẩn gồm 3 bước: **Search → Pull → Fix Permissions**

### 3.1. Bước 1: Tìm Kiếm (Search)

Tìm tên hoặc ID của thiết bị trong kho lưu trữ.

```bash
ishare2 search <type> [keyword]
```

**Các loại type:**
- `qemu` - Máy ảo QEMU (Router, Firewall, Windows...)
- `iol` - Cisco IOL (Router L3, Switch L2)
- `dynamips` - Cisco Dynamips (Router cũ)

**Keyword:** Tùy chọn, có thể bỏ qua để liệt kê tất cả.

#### 📝 Ví Dụ 1: Tìm Switch IOL

Bạn muốn tìm image của switch, nằm trong nhóm IOL, nhưng không nhớ rõ tên: 

```bash
ishare2 search iol
```

**ishare2** sẽ tìm và liệt kê tất cả image IOL cho bạn:

![image.png](https://github.com/user-attachments/assets/03064284-5860-4641-847f-0dde390f1b1b)

#### 📝 Ví Dụ 2: Tìm Fortinet Firewall

Bạn muốn tìm image của firewall Fortinet FGT, nhưng không biết nằm trong nhóm nào:

```bash
 ishare2 search FGT
```

**ishare2** sẽ tìm trong tất cả các type và liệt kê cho bạn. Bạn sẽ biết được Fortigate FGT nằm trong nhóm **QEMU**:

![image.png](https://github.com/user-attachments/assets/2c0fa8be-7906-479b-a91b-8647b59bb17e)

### 3.2. Bước 2: Tải Về (Pull)

Sau khi tìm được image, hãy ghi nhớ:

- **Type** của image: `qemu`, `iol`, hoặc `dynamips`
- **ID** của image (con số ở cột đầu tiên)

**Cú pháp lệnh:**

```bash
ishare2 pull <type> <id>
```

#### 📝 Ví Dụ: Tải Cisco Router IOL L3

Bạn muốn tải image **Cisco Router IOL L3**. Từ kết quả tìm kiếm, bạn biết:
- Type: **IOL**
- ID: **3** (L3-ADVENTERPRISEK9-M-15.4-2T.bin) 

![image.png](https://github.com/user-attachments/assets/f678be53-c5c8-4134-afe1-751cdad15aaa)

Chạy lệnh:

```bash
ishare2 pull iol 3
```

✅ Thông báo như bên dưới là hoàn tất. Image đã được tải về và có thể sử dụng:

![image.png](https://github.com/user-attachments/assets/47b600e3-2e18-49aa-bf68-59314a754cc8)

⚠️ **Lưu ý:** Đối với image **IOL**, bạn cần tạo file license (xem [Phần 4](#4-xử-lý-lỗi-license-cisco-iol-python-3-fix))

### 3.3. Bước 3: Sửa Quyền Hệ Thống (Fix Permissions)

Thông thường khi pull image về hệ thống sẽ tự động **Fix Permissions** như bước ở trên. Nếu vì một lý do gì mà bị lỗi thì nên chạy lệnh này sau mỗi lần tải để EVE-NG nhận diện và khởi động được thiết bị.

```bash
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

---

## 4. XỬ LÝ LỖI LICENSE CISCO IOL (PYTHON 3 FIX)

### 4.1. Giới Thiệu Vấn Đề

⚠️ **Vấn đề:** Image IOL (Router L3, Switch L2) yêu cầu file license để chạy. Không có license sẽ gặp lỗi:
- EVE báo: **"Cannot start the device"**
- Log hiển thị: **"No license found"**

Trên các bản EVE-NG mới, Python 2 đã bị loại bỏ khiến lệnh `ishare2 relicense` không hoạt động. Do đó, cần tạo license thủ công bằng Python 3.

### 4.2. Bước 1: Cài Đặt Python

Cài đặt Python 3:

```bash
apt install -y python-is-python3
```

### 4.3. Bước 2: Tạo File License Generator

#### 2.1. Tạo File keygen.py

```bash
touch keygen.py
```

#### 2.2. Chỉnh Sửa File

Mở file bằng vi editor:

```bash
vi keygen.py
```

Bấm phím **i** để vào chế độ Insert, sau đó paste nội dung sau (click chuột phải để paste): 

```bash
#! /usr/bin/python
print("*********************************************************************")
print("Cisco IOU License Generator - Kal 2011, python port of 2006 C version")
print("Modified to work with python3 by c_d 2014")
import os
import socket
import hashlib
import struct

# get the host id and host name to calculate the hostkey
hostid=os.popen("hostid").read().strip()
hostname = socket.gethostname()
ioukey=int(hostid,16)
for x in hostname:
 ioukey = ioukey + ord(x)
print("hostid=" + hostid +", hostname="+ hostname + ", ioukey=" + hex(ioukey)[2:])

# create the license using md5sum
iouPad1 = b'\x4B\x58\x21\x81\x56\x7B\x0D\xF3\x21\x43\x9B\x7E\xAC\x1D\xE6\x8A'
iouPad2 = b'\x80' + 39*b'\0'
md5input=iouPad1 + iouPad2 + struct.pack('!i', ioukey) + iouPad1
iouLicense=hashlib.md5(md5input).hexdigest()[:16]

print("\nAdd the following text to ~/.iourc:")
print("[license]\n" + hostname + " = " + iouLicense + ";\n")
print("You can disable the phone home feature with something like:")
print(" echo '127.0.0.127 xml.cisco.com' >> /etc/hosts\n")
```

**Lưu file:**
1. Bấm **ESC**
2. Gõ **:wq!**
3. Bấm **Enter**

### 4.4. Bước 3: Tạo License

Chạy script để tạo license:

```bash
python3 keygen.py
```

**Kết quả:** Script sẽ hiển thị thông tin license. Copy phần được tô sáng như hình:

![image.png](https://github.com/user-attachments/assets/270f5c10-22a6-4a22-99dc-7596ff4d5925)

### 4.5. Bước 4: Tạo File iourc

Tạo và chỉnh sửa file **iourc**:

```bash
touch iourc
vi iourc
```

Bấm **i** để vào chế độ Insert, sau đó paste license đã copy ở bước trước. Nội dung file **iourc** sẽ trông như sau:

![image.png](https://github.com/user-attachments/assets/c818149d-63f7-4815-bd1b-98c24dbca052)

**Lưu file:**
1. Bấm **ESC**
2. Gõ **:wq!**
3. Bấm **Enter**

### 4.6. Bước 5: Áp Dụng License

Copy file license vào thư mục chứa image IOL:

```bash
cp iourc /opt/unetlab/addons/iol/bin/
```

✅ **Hoàn tất!** Bây giờ các thiết bị IOL đã có thể khởi động bình thường.

---

## 5. DANH SÁCH IMAGE KHUYẾN NGHỊ (BEST PRACTICES)

🎯 Để đảm bảo Lab chạy mượt, ít tốn tài nguyên, khuyến nghị sử dụng các phiên bản sau:

| **Loại thiết bị** | **Phiên bản khuyến nghị (ID/Tên)** | **Lý do chọn** |
| --- | --- | --- |
| **Cisco Router** | **IOL L3 17.12.1** (hoặc 15.7) | Thay thế hoàn toàn Dynamips c7200 cũ kỹ. Nhẹ, khởi động nhanh. |
| **Cisco Switch** | **IOL L2 17.12.1** | Đầy đủ tính năng Layer 2, chạy ổn định. |
| **Windows PC** | **Windows 10 Enterprise** (ID `1722`) | Bản chuẩn quốc tế (Tiếng Anh), cân bằng giữa hiệu năng và tính năng. |
| **Windows (Yếu)** | **Tiny 10** (ID `1740`) | Siêu nhẹ, dùng để test ping/web cơ bản. |
| **FortiGate** | **v7.0.13** (ID `532`) | Bản ổn định nhất còn giữ cơ chế Trial 15 ngày không giới hạn tính năng. |

---

## 6. QUẢN LÝ VÀ TỐI ƯU HÓA TÀI NGUYÊN

### 6.1. 🗑️ Xóa Image Cũ (Giải Phóng Ổ Cứng)

Ví dụ xóa các file Dynamips c7200 cũ:

```bash
rm -rf /opt/unetlab/addons/dynamips/c7200*
```

### 6.2. ⚙️ Cấu Hình Node Windows Trên Web EVE

Để Windows chạy mượt mà, khi Add Node cần cấu hình tối thiểu:

- **CPU:** 2 vCPU
- **RAM:** 4096 MB (4GB)
- **Lưu ý:** Bật **"Virtualize Intel VT-x/EPT"** trong cài đặt VMware của máy ảo EVE-NG

### 6.3. 🌐 Xử Lý Image Ngôn Ngữ Trung Quốc

Nếu lỡ tải các bản Windows repack tiếng Trung:

1. Vào **Settings** → **Time & Language** → **Language**
2. Add language → Tìm **"English (United States)"**
3. Move **"English"** lên đầu danh sách (Top priority)
4. Sign out và đăng nhập lại

💡 **Khuyến nghị:** Nên tải lại bản ID 1722 tiếng Anh chuẩn để tránh lỗi font/tool.

---

## 📞 Hỗ Trợ

Nếu gặp vấn đề trong quá trình sử dụng ishare2:

### Các vấn đề thường gặp:
- **Không tải được image:** Kiểm tra kết nối Internet và DNS
- **Image không khởi động:** Chạy lệnh Fix Permissions
- **IOL báo lỗi license:** Thực hiện lại [Phần 4](#4-xử-lý-lỗi-license-cisco-iol-python-3-fix)
- **Hết dung lượng ổ cứng:** Xóa các image không dùng đến

### Tài nguyên tham khảo:
1. [ishare2 GitHub Repository](https://github.com/ishare2-org/ishare2-cli)
2. [EVE-NG Community Forum](https://www.eve-ng.net/index.php/community/)
3. [EVE-NG Documentation](https://www.eve-ng.net/index.php/documentation/)

### Báo lỗi:
- Tạo issue trên repository này
- Cung cấp thông tin chi tiết về lỗi gặp phải
- Đính kèm screenshot nếu có thể

---

## 📄 License

Hướng dẫn này được tạo ra cho mục đích chia sẻ kiến thức và hỗ trợ cộng đồng.

---

## 📚 Tài Nguyên Liên Quan

- [Hướng Dẫn Cài Đặt EVE-NG](../01-Installation/README.md)
- [EVE-NG Official Website](https://www.eve-ng.net/)
- [ishare2 CLI Tool](https://github.com/ishare2-org/ishare2-cli)

---

**⭐ Nếu hướng dẫn này hữu ích, đừng quên star repo này!**

---

<div align="center">

[↑ Về đầu trang](#hướng-dẫn-cài-đặt-ishare2-cho-eve-ngpnetlab)

</div>