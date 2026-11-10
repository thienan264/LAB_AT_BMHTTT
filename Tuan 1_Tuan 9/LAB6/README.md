Họ và tên: Trần Phạm Thiên Ân
MSSV: 1150080127
Tên bài LAB: Lab6 - Xác thực Active Directory và chính sách truy cập trên pfSense

Nội dung đã thực hiện:

- Bài lab thực nghiệm việc xác thực tài khoản Active Directory trên pfSense bằng LDAP và Captive Portal, sau đó cấu hình các chính sách truy cập theo IP, dịch vụ và thời gian.
- Sử dụng Mac M1 và phần mềm UTM để chạy các máy ảo gồm pfSense, Windows Server DC01, Client-A, Client-B và Ubuntu DMZ-WEB.
- Cấu hình pfSense với 4 interface WAN, MGMT, LAN và DMZ. Máy Mac quản trị pfSense qua địa chỉ 192.168.56.254.
- Tạo domain lab6.test, OU LAB6-Users, hai tài khoản sv_a, sv_b và tài khoản svc_pfsense_ldap để kết nối LDAP.
- Cấu hình Ubuntu chạy Nginx với các IP 172.16.6.10, .20, .30, .40 để mô phỏng các nhóm website.
- Tạo Alias, Schedule và thực nghiệm các kịch bản chặn website, giới hạn dịch vụ trong giờ hành chính và cô lập DMZ khỏi LAN.
- Kiểm tra kết quả bằng trình duyệt, lệnh curl, ping và Firewall Log.

Kết quả thực hiện:

- Kết nối các máy vào đúng mạng LAN và DMZ, truy cập được WebGUI pfSense từ máy Mac.
- Xác thực LDAP thành công với cả sv_a và sv_b. Client-B đăng nhập Captive Portal và tạo được phiên kết nối.
- Client-A truy cập được GENERAL_SITE nhưng bị chặn JOB_SITE.
- Cả Client-A và Client-B bị chặn RESTRICTED_SITE nhưng vẫn truy cập được GENERAL_SITE trong kịch bản 2.
- Client-B truy cập được SOCIAL_SITE khi TEST_NOW đang active và bị chặn khi lịch không còn active.
- Kiểm thử HTTP và HTTPS thành công trên cả hai client trong giờ thử. Ping tới DMZ không nhận được phản hồi; ngoài lịch, cả hai client không truy cập được GENERAL_SITE trong kịch bản 4.
- Trong kịch bản 5, Client-A vẫn truy cập được web DMZ, còn Ubuntu không ping được DC01. Firewall Log ghi nhận gói ICMP từ 172.16.6.10 tới 10.6.0.10 bị chặn.
- Chưa có minh chứng đầy đủ cho ruleset tổng hợp phần H, phép thử Reject và State Table.

Các lưu ý cần thiết để giảng viên có thể kiểm tra hoặc chạy lại bài:

- Bài thực hiện bằng UTM trên Mac M1. pfSense và Windows Server chạy bản x64 bằng giả lập; Windows 11 và Ubuntu dùng bản ARM64.
- Giữ đúng mạng lab6-lan cho DC01 và hai client; mạng lab6-dmz cho Ubuntu. Các máy này chỉ dùng một card mạng theo mô hình.
- DC01 dùng IP 10.6.0.10, Client-A dùng 10.6.0.21, Client-B dùng 10.6.0.22. Hai client dùng gateway 10.6.0.1 và DNS 10.6.0.10.
- Ubuntu dùng gateway 172.16.6.1, có đủ bốn IP mô phỏng và dịch vụ Nginx đang chạy.
- Bật pfSense và DC01 khi kiểm tra LDAP. Bật thêm client và Ubuntu khi kiểm tra truy cập website.
- Kiểm tra giờ và múi giờ pfSense trước khi thử Schedule. TEST_NOW là lịch thử tạm, cần chỉnh lại để chứa thời điểm kiểm tra.
- Sau khi sửa rule cần Apply Changes, tạo kết nối mới hoặc xóa state liên quan. Kiểm tra thứ tự rule để tránh Pass rộng nằm trên rule Block.
- Các ảnh được chụp theo từng kịch bản với cấu hình khác nhau, không phải toàn bộ cùng chạy trên một ruleset tổng hợp.