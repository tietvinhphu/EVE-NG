![EVE-NG & PNETLAB Banner](01-Installation/images/EVE-NG.png)

# 🌐 EVE-NG - Hướng Dẫn Toàn Diện

Repository này cung cấp hướng dẫn chi tiết về cài đặt, cấu hình và xử lý sự cố cho EVE-NG - hai nền tảng mô phỏng mạng phổ biến nhất cho Network Engineers.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/yourusername/eve-ng-guide.svg)](https://github.com/yourusername/eve-ng-guide/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/yourusername/eve-ng-guide.svg)](https://github.com/yourusername/eve-ng-guide/network)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/yourusername/eve-ng-guide/pulls)

---

## 📚 Mục Lục

- [Giới Thiệu](#-giới-thiệu)
- [Hướng Dẫn Chi Tiết](#-hướng-dẫn-chi-tiết)
- [Yêu Cầu Hệ Thống](#-yêu-cầu-hệ-thống)
- [Quick Start](#-quick-start)
- [Features](#-features)
- [Đóng Góp](#-đóng-góp)
- [License](#-license)
- [Liên Hệ](#-liên-hệ--hỗ-trợ)

---

## 📖 Giới Thiệu

### EVE-NG là gì?

**EVE-NG** (Emulated Virtual Environment - Next Generation) là nền tảng mô phỏng mạng mạnh mẽ cho phép bạn:
- Xây dựng các lab mạng phức tạp trong môi trường ảo hóa
- Thực hành với các thiết bị Cisco, Juniper, Fortinet, Palo Alto, và nhiều vendor khác
- Chuẩn bị cho các chứng chỉ như CCNA, CCNP, CCIE
- Kiểm thử và phát triển giải pháp mạng

---

## 🚀 Hướng Dẫn Chi Tiết

Repository này bao gồm 4 phần hướng dẫn chính:

### 1. 📦 [Hướng Dẫn Cài Đặt EVE-NG](./01-Installation)

Hướng dẫn từng bước cài đặt EVE-NG từ đầu:
- Cài đặt VMware Workstation Pro
- Download và cài đặt EVE-NG Community Edition
- Cấu hình máy ảo
- Cấu hình ban đầu và truy cập Web Interface
- Tips & Tricks tối ưu hóa

**👉 [Xem hướng dẫn đầy đủ](./01-Installation/README.md)**

---

### 2. 💾 [Hướng Dẫn Import Máy Ảo EVE Có Sẵn](./02-Import-VM)

Hướng dẫn import máy ảo EVE-NG/PNETLAB đã được cấu hình sẵn:
- Download pre-built VM
- Import vào VMware Workstation
- Cấu hình Network Adapter
- Khởi động và kiểm tra
- Troubleshooting các lỗi thường gặp

**👉 [Xem hướng dẫn đầy đủ](./02-Import-VM/README.md)**

---

### 3. 🖼️ [Hướng Dẫn Cài iShare2 - Lấy Image cho EVE/PNETLAB](./03-iShare2-Images)

Hướng dẫn sử dụng iShare2 để download network device images:
- Cài đặt và cấu hình iShare2
- Download images cho Cisco, Juniper, Fortinet, v.v.
- Upload images lên EVE-NG/PNETLAB
- Verify và test images
- Best practices cho quản lý images

**👉 [Xem hướng dẫn đầy đủ](./03-iShare2-Images/README.md)**

---

### 4. 🔧 [Hướng Dẫn Xử Lý Lỗi Thường Gặp](./04-Troubleshooting)

Tổng hợp các lỗi phổ biến và cách khắc phục:
- Lỗi kết nối mạng
- Lỗi không khởi động được node
- Lỗi Console không hoạt động
- Lỗi import/export lab
- Lỗi hiệu năng và tối ưu hóa
- FAQs

**👉 [Xem hướng dẫn đầy đủ](./04-Troubleshooting/README.md)**

---

## 💻 Yêu Cầu Hệ Thống

### Tối Thiểu

| Thành Phần | Yêu Cầu |
|------------|---------|
| **CPU** | Intel/AMD với 4 cores |
| **RAM** | 8GB |
| **Storage** | 50GB SSD |
| **OS** | Windows 10/11, macOS, Linux |
| **Virtualization** | VMware Workstation 15.x+ hoặc VirtualBox 6.x+ |

### Khuyến Nghị

| Thành Phần | Yêu Cầu |
|------------|---------|
| **CPU** | Intel i5/i7 hoặc AMD Ryzen 5/7 (6+ cores) |
| **RAM** | 16GB - 32GB |
| **Storage** | 100GB+ NVMe SSD |
| **OS** | Windows 11 hoặc Ubuntu 22.04 LTS |
| **Virtualization** | VMware Workstation Pro 17.x |

### Lưu Ý Quan Trọng

⚠️ **Enable Virtualization**: Đảm bảo VT-x/AMD-V được bật trong BIOS

⚠️ **Nested Virtualization**: Cần thiết nếu chạy EVE-NG trong VM

---

## ⚡ Quick Start

### Người Mới Bắt Đầu

Nếu bạn hoàn toàn mới với EVE-NG/PNETLAB:

1. 📖 Đọc phần [Giới Thiệu](#-giới-thiệu) để hiểu khái niệm cơ bản
2. 💻 Kiểm tra [Yêu Cầu Hệ Thống](#-yêu-cầu-hệ-thống)
3. 📦 Làm theo [Hướng Dẫn Cài Đặt](./01-Installation/README.md)
4. 🖼️ Tải images theo [Hướng Dẫn iShare2](./03-iShare2-Images/README.md)

### Người Có Kinh Nghiệm

Nếu bạn đã quen với virtualization:

1. 💾 Import VM có sẵn theo [Hướng Dẫn Import](./02-Import-VM/README.md)
2. 🖼️ Setup images với [iShare2](./03-iShare2-Images/README.md)
3. 🚀 Bắt đầu xây dựng lab của bạn

### Xử Lý Sự Cố

Gặp vấn đề? Xem [Troubleshooting Guide](./04-Troubleshooting/README.md)

---

## 🤝 Đóng Góp

Chúng tôi rất hoan nghênh mọi đóng góp từ cộng đồng!

### Đóng Góp Có Thể Bao Gồm

- 📝 Cải thiện documentation
- 🐛 Báo cáo và sửa lỗi
- 💡 Đề xuất tính năng mới
- 🖼️ Thêm screenshots/videos
- 🌍 Dịch sang ngôn ngữ khác
- 📚 Thêm use cases và examples

### Guidelines

- Tuân thủ coding style hiện tại
- Cập nhật documentation khi cần
- Test kỹ trước khi submit PR
- Viết commit messages rõ ràng

---

## 🎓 Tài Nguyên Học Tập

### Official Documentation
- [EVE-NG Official Docs](https://www.eve-ng.net/index.php/documentation/)

### Community
- [EVE-NG Forum](https://www.eve-ng.net/index.php/community/)
- [Reddit r/networking](https://www.reddit.com/r/networking/)
- [Discord Network Engineers](https://discord.gg/networking)

---

## 📞 Liên Hệ & Hỗ Trợ

### Issues & Bug Reports
Gặp vấn đề? [Tạo Issue](https://github.com/yourusername/eve-ng-pnetlab-guide/issues)

### Discussions
Có câu hỏi? [Join Discussions](https://github.com/yourusername/eve-ng-pnetlab-guide/discussions)

### Social Media
- 📧 Email: [tietvinhphu@gmail.com]
- 💼 LinkedIn: [Tiet Vinh Phu](https://www.linkedin.com/in/tiet-vinh-phu-609173155/)

---

## 🙏 Credits & Acknowledgments

Cảm ơn đến:
- **Kai Network** - Vì đã chia sẻ kiến thức
- **EVE-NG Team** - Vì đã tạo ra nền tảng tuyệt vời này
- **Community Contributors** - Vì đã đóng góp và hỗ trợ

---

## ⭐ Star History

Nếu repository này hữu ích cho bạn, đừng quên star ⭐ để ủng hộ nhé!

[![Star History Chart](https://api.star-history.com/svg?repos=yourusername/eve-ng-pnetlab-guide&type=Date)](https://star-history.com/#yourusername/eve-ng-pnetlab-guide&Date)

---

## 🔖 Tags

`eve-ng` `pnetlab` `network-simulation` `cisco` `ccna` `ccnp` `networking` `vmware` `virtualization` `lab-setup` `network-engineer` `juniper` `fortinet` `palo-alto` `network-automation`

---

<div align="center">

**Được tạo bởi Tiết Vinh Phú ❤️ cho các bạn sinh viên của FPT Jetking**

[⬆ Về đầu trang](#-eve-ng---hướng-dẫn-toàn-diện)

</div>
