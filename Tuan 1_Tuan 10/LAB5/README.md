Họ và tên: Trần Phạm Thiên Ân

MSSV: 1150080127

Tên bài LAB: Lab1 - Examining SSH Telnet in Wireshark

NỘI DUNG ĐÃ THỰC HIỆN

Cấu hình môi trường mạng
Gồm 1 máy Server là Ubuntu Server và 1 máy Window có cài đặt PuttY và Wireshark để vừa đóng vai trò là máy Client và máy Attacker.

Quá trình thực nghiệm
Bài lab thực nghiệm việc gửi gói tin giữa các máy cùng chung mạng qua lần lượt các giao thức Telnet và SSH và tiến hành bắt gói tin để kiểm tra từng giao thức.

Quá trình phân tích
Thực nghiệm việc thu thập thông tin đăng nhập qua các gói tin và phân tích dữ liệu trao đổi giữa Client và Server dùng Analyze - Follow > TCP stream để quan sát toàn bộ phiên TCP để thu thập và so sánh dữ liệu nhận được giữa Telnet và SSH.

KẾT QUẢ THỰC HIỆN

Kết nối mạng
Kết nối các máy chung cùng một mạng thành công.

Bắt gói tin
Thành công bắt được gói tin với Wireshark khi dữ liệu được gửi qua các cổng Telnet và SSH.

Thu thập dữ liệu
Kiểm tra và thu tập thông tin từ các gói tin bắt được từ từng cổng Telnet và SSH.

Đánh giá tính khả thi
Kiểm tra được độ hiệu quả và tính khả thi giữa Telnet và SSH.

CÁC LƯU Ý CẦN THIẾT (DÀNH CHO GIẢNG VIÊN)

Yêu cầu máy ảo
Thầy cô cần có 1 máy ảo Server Ubuntu và 1 Máy window11.

Yêu cầu kết nối mạng
Đảm bảo các máy cùng chung một mạng và ping được cho nhau.

Yêu cầu dịch vụ (Port 22, 23)
Kiểm tra các cổng 22 và 23 xem có đang ở trạng thái Listen hay chưa. Nếu không có thì thầy cô cần cài đặt Telnet và OpenSSH.

Yêu cầu phần mềm
Cài đặt Wireshark và PuttY trên máy Window11.