# He-thong-mang-Cong-ty-Kiem-toan
Thiết kế mạng cho công ty dịch vụ kiểm toán 3 tầng 
## 🏢 1. Tổng Quan Dự Án & Nhu Cầu Doanh Nghiệp
Dự án tập trung vào việc thiết kế và cấu hình hạ tầng mạng toàn diện cho một doanh nghiệp hoạt động trong lĩnh vực **Kiểm toán & Tư vấn Tài chính**. Do tính chất đặc thù của ngành kiểm toán dữ liệu cực kỳ nhạy cảm, hệ thống mạng được xây dựng nhằm đáp ứng các tiêu chuẩn khắt khe về tính ổn định, hiệu năng cao và bảo mật nghiêm ngặt.

### Quy hoạch mặt bằng trụ sở (3 Tầng):
- **Tầng 1:** Sảnh lễ tân, Phòng tư vấn, Phòng Hành chính, Khách.
- **Tầng 2:** Phòng Giám đốc, Phòng Họp, Phòng Pháp lý, Phòng IT & Server.
- **Tầng 3:** Phòng Kế toán, Phòng Thuế, Phòng Kiểm toán, Phòng Ăn & Kho.

---

## 📐 2. Kiến Trúc Hạ Tầng Mạng Triển Khai
Hệ thống được thiết kế theo mô hình **Kiến trúc Module Doanh nghiệp 3 lớp (Hierarchical 3-Layer):**
- **Lớp Lõi (Core):** Sử dụng Multilayer Switch tốc độ cao, xử lý định tuyến Inter-VLAN và đảm bảo dự phòng tối đa.
- **Lớp Phân phối (Distribution):** Thực thi các chính sách Access List (ACL), định tuyến và quản lý chất lượng dịch vụ QoS.
- **Lớp Truy cập (Access):** Kết nối trực tiếp đến các thiết bị đầu cuối (PC, IP Phone, Wifi) qua Switch Layer 2.

### Phân vùng Module chức năng:
- **ISP Edge:** Kết nối Leased Line từ nhà mạng qua đường truyền CSU/DSU kết nối tới Router biên.
- **Vùng DMZ (Demilitarized Zone):** Chứa các Server công cộng phục vụ trao đổi bên ngoài (Mail Server, Web Server, DNS Server).
- **Vùng Server Farm:** Lưu trữ an toàn File Server nội bộ và Database của công ty.
- **Vùng Management:** Quản trị tập trung với DHCP Server và Domain Controller (DC).

---

## 🛠️ 3. Giải Pháp Quản Trị & Cấu Hình Kỹ Thuật
Dự án được cấu hình và kiểm thử thành công trên môi trường giả lập **Cisco Packet Tracer** với các thiết bị: Router 2811, Switch 2960, Multilayer Switch 3560 và các dòng Server chuyên dụng.

### Các công nghệ cốt lõi đã triển khai:
1. **VLAN & VTP:** Chia nhỏ mạng logic theo từng phòng ban đặc thù để giảm miền quảng bá (Broadcast domain) và nâng cao bảo mật. Đồng bộ hóa cơ sở dữ liệu VLAN qua giao thức VTP.
2. **Inter-VLAN Routing:** Định tuyến trực tiếp giữa các phòng ban tại Core Switch Layer 3 nhằm tối ưu hóa băng thông nội bộ.
3. **Routing & WAN:** Cấu hình giao thức định tuyến động **OSPF** giữa Router biên và Core Switch.
4. **NAT Overload (PAT):** Chuyển đổi địa chỉ IP nội bộ sang IP công cộng tại mặt ngoài của Router biên để tối ưu hóa địa chỉ và cho phép toàn bộ công ty ra Internet.
5. **Dịch vụ mạng:** Cấu hình **DHCP Relay Agent** tại Core Switch trỏ về máy chủ DHCP tập trung giúp cấp phát IP tự động cho các tầng. Kiểm thử thành công phân giải tên miền (DNS), dịch vụ Web và Mail nội bộ.

---

## 🔒 4. Chính Sách An Ninh Mạng & Phân Quyền (Access Control List)
Để bảo vệ dữ liệu tài chính - kế toán và tuân thủ các quy định bảo mật, hệ thống áp dụng các chính sách **Access List (ACL)** nghiêm ngặt:
- **Bảo vệ vùng Server:** Chỉ duy nhất phòng IT được phép Ping quản trị vào các thiết bị thuộc vùng DMZ và Server Farm.
- **Cách ly phòng Kế toán:** Phòng Kế toán bị chặn kết nối Internet trực tiếp nhằm ngăn ngừa nguy cơ rò rỉ số liệu tài chính hoặc bị mã hóa dữ liệu bởi mã độc bên ngoài.
- **Phân quyền Máy khách (Guest):** Các máy thuộc VLAN Khách chỉ được phép truy cập Internet và Website trung tâm của công ty, hoàn toàn bị chặn kết nối tới tất cả các vùng mạng nội bộ khác.
