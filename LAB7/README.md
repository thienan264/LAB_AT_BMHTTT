Họ và tên: Trần Phạm Thiên Ân
MSSV: 1150080127
Tên bài LAB: Lab7 - Xây dựng chính sách Web Protection trên mô hình pfSense + Web Proxy

Nội dung thực hiện:

- Bài lab thực nghiệm việc kiểm soát truy cập web bằng pfSense kết hợp với Web Proxy.
- Sử dụng lại mô hình mạng LAB6 gồm pfSense, DC01, Client-A, Client-B và DMZ-WEB.
- Cài thêm máy Ubuntu Server làm WEB-PROXY, sử dụng Squid, E2Guardian và ClamAV để xử lý truy cập và lọc nội dung web.
- Cấu hình địa chỉ IP, DNS và các website mô phỏng để phục vụ việc kiểm thử.
- Cấu hình trình duyệt trên các máy Client sử dụng proxy và thiết lập firewall rule để chặn truy cập web trực tiếp.
- Thực nghiệm các chính sách chặn website, cảnh báo và cho phép truy cập tạm thời, ngoại lệ và giới hạn thời gian truy cập.
- Kiểm tra quét file tải xuống bằng ClamAV và theo dõi log để đối chiếu kết quả.

Kết quả thực hiện:

- Các máy kết nối đúng mô hình mạng, phân giải được tên miền và truy cập được các website mô phỏng.
- Client truy cập web được khi sử dụng proxy và bị chặn truy cập trực tiếp khi tắt proxy.
- Các website thuộc danh sách chặn không truy cập được.
- Các website thuộc nhóm cảnh báo hiển thị cảnh báo và cho phép truy cập tạm thời.
- Các website thuộc danh sách ngoại lệ truy cập bình thường.
- Chính sách thời gian cho kết quả khác nhau trong và ngoài khung giờ cho phép.
- File kiểm thử EICAR bị phát hiện khi tải qua proxy đã bật quét nội dung.
- Kiểm tra log trên pfSense, E2Guardian và Squid để xác định nguyên nhân cho phép hoặc chặn truy cập.

Các lưu ý cần thiết để giảng viên có thể kiểm tra hoặc chạy lại bài:

- Cần có các máy ảo pfSense, DC01, Client-A, Client-B, DMZ-WEB và WEB-PROXY.
- Đảm bảo các máy được nối đúng mạng LAN và DMZ theo mô hình bài lab.
- WEB-PROXY sử dụng IP 10.6.0.30/24, Gateway 10.6.0.1 và DNS 10.6.0.10.
- Cấu hình Manual Proxy trên Client-A và Client-B là 10.6.0.30, cổng 8080.
- Kiểm tra các dịch vụ Squid, E2Guardian, ClamAV và Nginx trước khi thực hiện các phép thử.
- Các tên miền lab7.test phải được phân giải về máy DMZ-WEB tại 172.16.6.10.
- Sau khi chỉnh firewall rule cần Apply Changes và xóa state cũ khi cần kiểm tra lại.
- Sử dụng website mô phỏng và file kiểm thử trong bộ tài nguyên của bài lab.
- Kết quả thực hiện cụ thể được trình bày trong báo cáo và các hình ảnh minh chứng đi kèm.