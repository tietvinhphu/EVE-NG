# Hướng dẫn cài ishare2 để lấy image cho EVE/PNETLAB

---

HƯỚNG DẪN CÀI **ISHARE2** ĐỂ LẤY IMAGE CHO EVE

Các bạn khi làm lab ảo với EVE dễ gặp vấn đề về TÌM và CÀI ĐẶT image cho thiết bị lên EVE. Để hỗ trợ điều này, có công cụ tên là **ishare2**, đó như một kho image, các bạn chỉ cần gõ lệnh là có thể tìm và cài đặt image mong muốn.

Hướng dẫn này có thể áp dụng cho cả PNETLAB

Github của **ishare2** : [https://github.com/ishare2-org/ishare2-cli](https://github.com/ishare2-org/ishare2-cli)

**Mục lục :**

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

SSH vào giao diện CLI của EVE (hoặc dùng luôn giao diện CLI trên máy ảo, nhưng SSH dễ làm hơn)

![image.png](https://github.com/user-attachments/assets/e8a04ac2-d869-4b95-a2c4-a472e027631a)

Do server trang chủ thường xuyên gián đoạn, sử dụng mã nguồn trực tiếp từ GitHub để đảm bảo tính ổn định.

**Lệnh cài đặt:**

```bash
wget -O /usr/sbin/ishare2 https://raw.githubusercontent.com/ishare2-org/ishare2-cli/main/ishare2 && chmod +x /usr/sbin/ishare2
```

![image.png](https://github.com/user-attachments/assets/f453c973-3d4c-4477-bbc4-b541a695e077)

Đoạn script sẽ tự động cài cái package cần thiết, sau đó cài **ishare2** vào EVE

![image.png](https://github.com/user-attachments/assets/9c8e79cf-c660-48d6-a234-645869114f1d)

Cấu hình cho **ishare2** :

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

Thông báo cài đặt **ishare2** hoàn tất

**Kiểm tra:**
Gõ lệnh `ishare2`. Nếu hiển thị menu Help là cài đặt thành công.

![image.png](https://github.com/user-attachments/assets/1e2f0e16-bcbc-42f7-8e47-b327e2c5b730)

---

## 3. QUY TRÌNH TẢI IMAGE CHUẨN (SOP)

Quy trình bắt buộc gồm 3 bước: **Search -> Pull -> Fix Permissions**.

### Bước 1: Tìm kiếm (Search)

Tìm tên hoặc ID của thiết bị trong kho lưu trữ.

```bash
ishare2 search <type> [keyword]
```

Trong đó **type** có thể là :

- qemu
- iol
- dynamips

Keyword có thể có hoặc không

Ví dụ bạn muốn tìm image của switch, nằm trong nhóm iol, mà bạn không nhớ rõ tên : 

Gõ lệnh : 

```bash
ishare2 search iol
```

**ishare2** sẽ tìm và liệt kê tất cả image iol cho bạn

![image.png](https://github.com/user-attachments/assets/03064284-5860-4641-847f-0dde390f1b1b)

Ví dụ 2, tìm image của firewall Fortinet FGT, mà bạn không biết nằm trong nhóm nào

```bash
 ishare2 search FGT
```

ishare2 sẽ tìm trong tất cả các type và liệt kê cho bạn. Lúc này bạn cũng sẽ biết được Fortigate FGT nằm trong nhóm QEMU

![image.png](https://github.com/user-attachments/assets/2c0fa8be-7906-479b-a91b-8647b59bb17e)

### Bước 2: Tải về (Pull)

Sau khi tìm image ở bước trước, bạn hãy lưu lại : 

- **Type** của image : là **qemu**, hay **iol**, hay **dynamips**
- **ID** của image (là con số ở cột đầu tiên)

Lệnh cài đặt một image : 

```bash
ishare2 pull <type> <id>
```

Ví dụ mình tìm image của **Cisco Router IOL L3**. Mình sẽ biết được L3 thuộc nhóm **IOL**, và mình chọn image ID số 3**. L3-ADVENTERPRISEK9-M-15.4-2T.bin** 

![image.png](https://github.com/user-attachments/assets/f678be53-c5c8-4134-afe1-751cdad15aaa)

Mình sẽ cài đặt bằng lệnh : 

```bash
ishare2 pull iol 3
```

Thông báo như bên dưới là hoàn tất, bạn có thể sử dụng luôn image mà KHÔNG CẦN phải fix permission. 

![image.png](https://github.com/user-attachments/assets/47b600e3-2e18-49aa-bf68-59314a754cc8)

Tuy nhiên, bạn vẫn phải :

- Tạo file license cho image **iol ⇒ Phần 4**

### Bước 3: Sửa quyền hệ thống (Fix Permissions)

Thông thường khi pull image về hệ thống sẽ tự động **Fix Permissions** như bước ở trên. Nếu vì một lý do gì mà bị lỗi thì nên chạy lệnh này sau mỗi lần tải để EVE-NG nhận diện và khởi động được thiết bị.

```bash
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

---

## 4. XỬ LÝ LỖI LICENSE CISCO IOL (PYTHON 3 FIX)

Nếu bạn dùng image iol (ví dụ như switch), bạn cần file license thì mới chạy được. Nếu không có file license, thì khi start thiết bị, EVE sẽ báo “cannot start the device”, trong log báo không có license.

Trên các bản EVE-NG mới, Python 2 đã bị loại bỏ khiến tính năng `ishare2 relicense` tự động bị lỗi. Cần tạo License thủ công bằng Script Python 3.

### Bước 1: Cài python

Chạy lệnh sau

```bash
apt install -y python-is-python3
```

### Bước 2. Tạo file License iourc cho image iol thủ công

Tạo file có tên **keygen.py**

```bash
touch keygen.py
```

Mở file 

```bash
vi keygen.py
```

Bấm phím **i** để edit

Copy and Paste nội dung bên dưới vào file **keygen.py** (paste bằng cách click chuột phải) : 

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

Lưu và thoát bằng cách :

- Bấm **ESC**
- Gõ **wq!**
- **Enter**

Chạy file code : 

```bash
python3 keygen.py
```

Copy phần license (tô sáng):

![image.png](https://github.com/user-attachments/assets/270f5c10-22a6-4a22-99dc-7596ff4d5925)

Tạo file **iourc** và dán phần đó vào

```bash
touch iourc
vi iourc
```

Bấm **i** để edit, và dán license vào, nội dung file **iourc** trông như sau

![image.png](https://github.com/user-attachments/assets/c818149d-63f7-4815-bd1b-98c24dbca052)

Lưu và thoát bằng cách :

- Bấm **ESC**
- Gõ **wq!**
- **Enter**

Copy file license vào thư mục chứa image **iol**

```bash
cp iourc /opt/unetlab/addons/iol/bin/
```

---

## 5. DANH SÁCH IMAGE KHUYẾN NGHỊ (BEST PRACTICES)

Để đảm bảo Lab chạy mượt, ít tốn tài nguyên, khuyến nghị sử dụng các phiên bản sau:

| **Loại thiết bị** | **Phiên bản khuyến nghị (ID/Tên)** | **Lý do chọn** |
| --- | --- | --- |
| **Cisco Router** | **IOL L3 17.12.1** (hoặc 15.7) | Thay thế hoàn toàn Dynamips c7200 cũ kỹ. Nhẹ, khởi động nhanh. |
| **Cisco Switch** | **IOL L2 17.12.1** | Đầy đủ tính năng Layer 2, chạy ổn định. |
| **Windows PC** | **Windows 10 Enterprise** (ID `1722`) | Bản chuẩn quốc tế (Tiếng Anh), cân bằng giữa hiệu năng và tính năng. |
| **Windows (Yếu)** | **Tiny 10** (ID `1740`) | Siêu nhẹ, dùng để test ping/web cơ bản. |
| **FortiGate** | **v7.0.13** (ID `532`) | Bản ổn định nhất còn giữ cơ chế Trial 15 ngày không giới hạn tính năng. |

---

## 6. QUẢN LÝ VÀ TỐI ƯU HÓA TÀI NGUYÊN

### 6.1. Xóa Image cũ (Giải phóng ổ cứng)

Ví dụ xóa các file Dynamips c7200 cũ:

```bash
rm -rf /opt/unetlab/addons/dynamips/c7200*
```

### 6.2. Cấu hình Node Windows trên Web EVE

Để Windows chạy mượt mà, khi Add Node cần cấu hình tối thiểu:

- **CPU:** 2 vCPU.
- **RAM:** 4096 MB (4GB).
- **Lưu ý:** Bật "Virtualize Intel VT-x/EPT" trong cài đặt VMware của máy ảo EVE-NG.

### 6.3. Xử lý Image ngôn ngữ Trung Quốc

Nếu lỡ tải các bản Windows repack tiếng Trung:

1. Vào Settings -> Time & Language -> Language.
2. Add language -> Tìm "English (United States)".
3. Move "English" lên đầu danh sách (Top priority).
4. Sign out và đăng nhập lại.
*(Khuyến nghị: Nên tải lại bản ID 1722 tiếng Anh chuẩn để tránh lỗi font/tool).*