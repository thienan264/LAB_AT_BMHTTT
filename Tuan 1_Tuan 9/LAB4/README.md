Họ và tên: Trần Phạm Thiên ÂnMSSV: 1150080127
Tên bài LAB: Lab 4 - Khảo sát và đánh giá bề mặt mạng bằng Nmap

- Nội dung đã thực hiện:

Bài lab thực nghiệm việc sử dụng công cụ Nmap để rà soát an toàn mạng nội bộ, phát hiện các host, cổng mở, dịch vụ và rủi ro cơ bản.   

Gồm 1 máy ảo Kali Linux đóng vai trò là máy quét chính.   

1 máy ảo Metasploitable 2 đóng vai trò là máy đích cố ý có lỗ hổng.   

Thực nghiệm các kỹ thuật quét TCP và UDP khác nhau (như SYN scan, TCP Connect, FIN, ACK) để so sánh cách hệ thống mục tiêu phản hồi và phân tích các trạng thái cổng (open, closed, filtered).   

Vận dụng các đối số của Nmap để nhận diện phiên bản dịch vụ, hệ điều hành và dùng NSE (Nmap Scripting Engine) để kiểm tra lỗ hổng bảo mật SMB.   

So sánh sự thay đổi của bề mặt mạng trước và sau khi thực hiện các biện pháp cấu hình phòng thủ (hardening).   

Thực hành việc xuất kết quả quét ra nhiều định dạng (normal text, xml, grepable) để tạo hồ sơ bằng chứng.   

- Kết quả thực hiện:

Thiết lập thành công môi trường mạng cách ly hoàn toàn để phục vụ việc quét lỗ hổng.   

Phát hiện thành công các thiết bị đang hoạt động trong mạng lưới Host-Only.   

Nhận diện chính xác hệ điều hành, các dịch vụ đang chạy và lỗ hổng trên máy Metasploitable 2.   Nắm rõ cách xuất và lưu trữ các báo cáo cấu hình bằng Nmap.   

- Các lưu ý cần thiết để giảng viên có thể kiểm tra hoặc chạy lại bài:

Bài được thực hành với máy thật là thiết bị Mac M1, sử dụng phần mềm ảo hóa UTM (hoặc VMware kết hợp UTM) thay cho VirtualBox.

Giảng viên cần có 1 máy ảo Kali Linux và 1 máy ảo Metasploitable 2.

Cần đảm bảo các máy ảo này phải được cấu hình kết nối chung vào một mạng Host-Only Network (ví dụ dải 192.168.56.0/24) và có thể ping được cho nhau.  

Máy quét Kali Linux cần được cài đặt sẵn công cụ Nmap.   

Tuyệt đối không cấu hình mạng của máy đích Metasploitable 2 ở chế độ Bridged ra mạng thật. 