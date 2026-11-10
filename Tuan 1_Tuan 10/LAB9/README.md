Họ và tên: Trần Phạm Thiên Ân
MSSV: 1150080127
Tên bài LAB: Lab 8 - Local DNS Attack: DNS Spoofing và Cache Poisoning trong mạng ảo cô lập

Nội dung thực hiện:

- Thiết lập phân đoạn mạng ảo cô lập DNS-LAB (10.9.0.0/24) trên tường lửa pfSense, cấu hình bộ 5 firewall rules bảo vệ hạ tầng nội bộ và chặn hoàn toàn kết nối ra ngoài Internet (WAN).
- Cài đặt và cấu hình máy chủ DNS-SERVER sử dụng BIND9 trên Ubuntu Server 24.04, đóng vai trò Authoritative cho zone lab9.test, phân giải nghịch cho dải 10.9.0.0/24 và chuyển tiếp (forward) tên miền external.lab9.test sang Authoritative DNS mô phỏng tại DMZ.
- Cấu hình máy VICTIM (Ubuntu) sử dụng Netplan để trỏ DNS duy nhất về DNS-SERVER (10.9.0.10) và thực hiện đo kiểm phân giải đường cơ sở (Baseline) trước khi tấn công.
- Khởi động dịch vụ DNS Authoritative mô phỏng (auth_dns_delayed.py) trên APP-TEST tại DMZ với độ trễ 1.5 giây để mô phỏng điều kiện chạy đua gói tin mạng.
- Thực nghiệm Task 1 - Thay đổi file /etc/hosts trên VICTIM: Chuyển hướng tên miền www.lab9.test về IP của Attacker, đối chiếu sự khác biệt giữa cơ chế NSS của ứng dụng (curl, browser) và công cụ chẩn đoán DNS chuyên dụng (dig).
- Thực nghiệm Task 2 - Direct DNS Spoofing: Sử dụng Kali Linux ở chế độ Promiscuous Mode trong cùng mạng L2, dùng Scapy bắt gói truy vấn của Victim và gửi phản hồi DNS giả mạo trực tiếp về Victim (khớp Transaction ID và UDP Port).
- Thực nghiệm Task 3 - DNS Cache Poisoning: Sử dụng Kali Linux giả mạo phản hồi từ Authoritative Server gửi về cho DNS-SERVER (BIND9), làm ngộ độc kho lưu trữ bộ nhớ đệm tập trung (cache) của Resolver với TTL 120 giây.
- Thực hiện các bước khôi phục môi trường (flush cache, khôi phục hosts) và thực nghiệm biện pháp phòng thủ Hardening bằng cách tái kích hoạt cơ chế DNS COOKIE (RFC 7873).

Kết quả thực hiện:

- Mạng DNS-LAB được pfSense định tuyến chính xác; các rule cô lập chặn thành công lưu lượng từ DNS-LAB sang các mạng nhạy cảm (MGMT, LAN-DOMAIN, WAN).
- DNS-SERVER phân giải chính xác forward/reverse zone; kết quả Baseline trước tấn công phân giải www.external.lab9.test trỏ đúng về website hợp lệ tại 172.16.6.10.
- Task 1: Lệnh curl trên Victim bị điều hướng sang trang web giả mạo (Fake Site) tại 10.9.0.200, trong khi lệnh dig @10.9.0.10 vẫn phản hồi chính xác IP gốc 172.16.6.10 do bỏ qua file /etc/hosts.
- Task 2: Script Scapy trên Kali bắt được truy vấn và phản hồi giả thắng cuộc đua; Victim truy cập domain bị chuyển hướng sang Fake Site của Kali; Wireshark ghi nhận gói tin giả mạo khớp hoàn toàn Transaction ID và UDP Destination Port của Victim.
- Task 3: BIND9 chấp nhận phản hồi giả mạo; lệnh dump cache (rndc dumpdb) ghi nhận bản ghi www.external.lab9.test trỏ về 10.9.0.200 với TTL 120s; các client trong mạng truy vấn đều nhận kết quả độc hại từ bộ nhớ đệm.
- Sau khi flush cache trên DNS-SERVER và Client, kết quả phân giải lập tức trở về địa chỉ IP hợp lệ 172.16.6.10; khi bật lại DNS COOKIE, các gói tin giả mạo bị BIND9 loại bỏ hoàn toàn.

Các lưu ý cần thiết để giảng viên có thể kiểm tra hoặc chạy lại bài:

- Cần có các máy ảo: pfSense (kế thừa LAB trước), DNS-SERVER (10.9.0.10), VICTIM (10.9.0.100), ATTACKER Kali (10.9.0.200), DMZ-WEB (172.16.6.10) và APP-TEST / AUTH-DNS (172.16.6.20).
- Interface DNSLAB (10.9.0.1/24) trên pfSense tuyệt đối không được cấu hình Upstream Gateway để tránh bị firewall nhận diện nhầm thành WAN interface.
- Máy Kali và các máy trong dải DNS-LAB phải kết nối cùng một mạng Host-Only (Internal Network: dns-lab); card mạng của Kali cần bật Promiscuous Mode (Allow VMs) để bắt được lưu lượng unicast giữa Victim và DNS-SERVER.
- Máy APP-TEST phải chạy script auth_dns_delayed.py với tham số --bind 172.16.6.20 --delay 1.5 để tạo race window.
- Trong pha tấn công, DNS-SERVER cấu hình send-cookie no đối với server 172.16.6.20 để cho phép mô phỏng thành công cuộc tấn công on-path.
- Sau mỗi kịch bản tấn công, cần thực hiện sudo rndc flush trên DNS-SERVER và sudo resolvectl flush-caches trên VICTIM để xóa sạch dữ liệu đệm cũ.
- Kết quả thực hiện cụ thể được trình bày trong báo cáo và các hình ảnh minh chứng đi kèm.