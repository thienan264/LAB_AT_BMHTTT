Họ và tên: Trần Phạm Thiên Ân
MSSV: 1150080127
Tên bài LAB: Lab 10 - Remote DNS Cache Poisoning trong môi trường pfSense cô lập

Nội dung thực hiện:

- Nâng cấp mô hình mạng từ LAB trước bằng cách bổ sung thêm phân đoạn mạng ATTACKNET (10.10.0.1/24) trên pfSense và tách máy ATTACKER (Kali) sang subnet độc lập này; thiết lập Promiscuous Mode = Deny để loại bỏ khả năng bắt lén (sniff) gói tin của DNS-SERVER (đảm bảo tính chất "Remote").
- Cấu hình ma trận firewall rules trên pfSense để phân quyền kết nối chặt chẽ giữa DNSLAB (10.9.0.0/24), ATTACKNET (10.10.0.0/24) và DMZ (172.16.6.0/24), cô lập hoàn toàn các mạng khỏi WAN và LAN-DOMAIN.
- Thiết lập Baseline phân giải tên miền hợp lệ www.example.lab10.test trên DNS-SERVER (BIND9) và dịch vụ Authoritative DNS/Relay (172.16.6.20), xác nhận Victim truy cập chính xác website hợp lệ tại 172.16.6.10.
- Thực nghiệm xác nhận tính "Remote": Sử dụng tcpdump trên Kali kiểm tra và chứng minh Kali hoàn toàn không nhìn thấy lưu lượng trao đổi DNS giữa DNS-SERVER và AUTH-DNS.
- Triển khai pha tấn công mô phỏng lỗ hổng Kaminsky: Tạm dừng BIND9 và kích hoạt resolver mô phỏng (vuln_resolver_lab10.py) cố định cổng nguồn UDP 33333; khởi động Evil DNS (evil_dns_lab10.py) và Fake Web Server trên Kali.
- Thực nghiệm Task 1 - Remote Cache Poisoning: Chạy attack script (kaminsky_remote_lab10.py) phát các query ngẫu nhiên để vượt qua bộ đệm (cache), gửi các batch dự đoán Transaction ID 16-bit tới Race Relay (UDP 5300) để giả mạo phản hồi chứa bản ghi ủy quyền NS (ns.evil.example.lab10.test) và Glue record trỏ về Kali.
- Thực nghiệm Task 2 - Phân tích bộ nhớ đệm và kết quả chuyển hướng: Kiểm chứng trạng thái [CACHE] delegation poisoned trên resolver; kiểm tra máy Victim bị chuyển hướng toàn bộ truy vấn của zone về máy chủ độc hại của Attacker.
- Thực hiện Hardening: Khôi phục dịch vụ BIND 9.18 hiện đại, thu hẹp ACL cho phép đệ quy (chỉ cho phép 10.9.0.0/24, tước quyền đệ quy của Kali), quan sát cơ chế ngẫu nhiên hóa cổng nguồn (Source-Port Randomization) bằng tcpdump và phân tích vai trò phòng thủ của DNSSEC.

Kết quả thực hiện:

- Phân đoạn mạng ATTACKNET và chính sách tường lửa trên pfSense hoạt động chuẩn xác; Kali hoàn toàn bị cô lập ở L2 và không bắt được gói tin của DNS-SERVER.
- Phân giải Baseline trước tấn công đạt yêu cầu: www.example.lab10.test phân giải chính xác về 172.16.6.10 và mở đúng trang web thật tại DMZ.
- Kịch bản tấn công Remote Cache Poisoning thành công: Race relay ghi nhận tín hiệu [HIT], resolver mô phỏng ghi nhận [CACHE] delegation poisoned thành công đối với zone example.lab10.test trỏ về NS độc hại 10.10.0.200.
- Sau khi bị ngộ độc, máy VICTIM truy cập www.example.lab10.test lập tức bị chuyển hướng về địa chỉ IP của Kali (10.10.0.200) và hiển thị trang web giả mạo LAB 10 Fake Site.
- Bắt và phân tích được bằng chứng gói tin: Upstream query từ cổng 33333, gói phản hồi giả mạo từ AUTH-DNS chứa bản ghi Authority NS và Additional Glue record khớp Transaction ID.
- Sau khi khôi phục BIND 9.18: Resolver hoạt động ổn định; tcpdump chứng minh BIND9 sử dụng cổng nguồn UDP ngẫu nhiên thay vì cố định 33333 (loại bỏ khả năng tấn công brute-force từ xa); quyền recursion của Kali bị chặn hoàn toàn.

Các lưu ý cần thiết để giảng viên có thể kiểm tra hoặc chạy lại bài:

- Cần có các máy ảo: pfSense, DNS-SERVER (10.9.0.10), VICTIM (10.9.0.100), ATTACKER Kali (10.10.0.200), DMZ-WEB (172.16.6.10) và APP-TEST / AUTH-DNS (172.16.6.20).
- pfSense cần thêm interface ATTACKNET (10.10.0.1/24) không gán Upstream Gateway; áp dụng đúng ma trận rule cho phép ATTACKNET gửi UDP 53 đến DNS-SERVER và UDP 5300 đến APP-TEST.
- Máy Kali phải được chuyển sang mạng riêng (attack-net), tháo bỏ card dns-lab cũ và đảm bảo Promiscuous Mode = Deny.
- Trên APP-TEST (172.16.6.20) cần chạy auth_dns_relay_lab10.py để vừa đóng vai trò Authoritative DNS, vừa làm race relay trên cổng 5300.
- Trong pha tấn công (Attack Phase), cần tắt BIND9 (sudo systemctl stop bind9) và chạy vuln_resolver_lab10.py trên DNS-SERVER; sau khi tấn công xong cần dừng script này và bật lại BIND9.
- Trên Kali cần mở sẵn 2 dịch vụ nền: evil_dns_lab10.py (port 53) và Fake HTTP Server (port 80).
- Sau khi hoàn thành bài lab, cần tắt rule UDP 5300 trên pfSense để đảm bảo an toàn hệ thống.
- Kết quả thực hiện cụ thể được trình bày trong báo cáo và các hình ảnh minh chứng đi kèm.