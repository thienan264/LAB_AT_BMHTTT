Họ và tên: Trần Phạm Thiên Ân
MSSV: 1150080127
Tên bài LAB: Lab 3: Nhận diện và ứng phó các mối đe dọa đến an toàn thông tin[cite: 5]

Nội dung đã thực hiện:
- Phân biệt bốn khái niệm cốt lõi: lỗ hổng (Vulnerability), mối đe dọa (Threat), rủi ro (Risk) và tấn công (Attack) trên một máy trạm Windows[cite: 5].
- Nhận diện năm nhóm nguồn đe dọa chính: hành động vô ý, hành động cố ý, thảm họa tự nhiên, lỗi kỹ thuật và lỗi quản lý[cite: 5].
- Thu thập bằng chứng cho các nhóm kỹ thuật tấn công như: mã độc, tấn công mật khẩu, keylogging, backdoor/persistence, DoS/DDoS, mail bombing, sniffing, man-in-the-middle, spoofing và social engineering/phishing[cite: 5].
- Cài đặt và sử dụng các công cụ phân tích gồm: Microsoft Defender, Windows Event Log, Sysmon, Autoruns, Process Explorer và Wireshark để thu thập bằng chứng từ endpoint và mạng[cite: 5].
- Thực hiện toàn diện quy trình ứng phó sự cố: Baseline – Observe – Detect – Contain – Recover – Verify dựa trên dữ liệu thử nghiệm thuộc phạm vi bài LAB[cite: 5].

Kết quả thực hiện:
- Lập risk register thành công, nhận diện và phân loại đúng các tình huống đe dọa giả định[cite: 5].
- Tạo thành công tệp kiểm thử EICAR để kiểm chứng quy trình phát hiện (detection) và cách ly (quarantine) của Microsoft Defender[cite: 5].
- Thu thập thành công các log sự kiện xác thực (Event ID 4624, 4625, 4648) và kiểm chứng tính hiệu quả của việc đổi mật khẩu[cite: 5].
- Phát hiện thành công cơ chế persistence (thông qua Run value, Scheduled Task) và dịch vụ lắng nghe (listener trên cổng 8080) bằng các công cụ Sysmon, Autoruns và Process Explorer[cite: 5].
- Sử dụng Wireshark bắt thành công lưu lượng HTTP loopback chứa chuỗi huấn luyện để phân tích sniffing, đồng thời so sánh tính bảo mật với giao thức HTTPS[cite: 5].
- Hoàn thành chạy thử nghiệm giới hạn DoS cục bộ, cũng như phân tích các tập dữ liệu offline về DDoS và Mail bombing[cite: 5].
- Nhận diện thành công các chỉ dấu độc hại qua mẫu phishing email offline và phân loại đúng các tình huống Social Engineering[cite: 5].
- Hoàn tất quy trình dọn dẹp (cleanup) các artefact thử nghiệm, xóa dịch vụ/tài khoản rác, xác nhận tính toàn vẹn của bằng chứng thu thập được bằng mã băm SHA-256 và khôi phục máy ảo về trạng thái ban đầu[cite: 5].

Các lưu ý cần thiết để giảng viên có thể kiểm tra hoặc chạy lại bài
- Môi trường ảo hóa: Cần sử dụng máy ảo Windows 11 (hỗ trợ cả bản x64 hoặc ARM64 cho máy Mac M1) chạy trên phần mềm VMware[cite: 5].
- Cấu hình mạng: Card mạng của máy ảo cần được thiết lập ở chế độ Host-only (hoặc Private to my Mac) làm mặc định để đảm bảo cô lập môi trường thực hành[cite: 5].
- Cấu hình bảo mật hệ thống: Tính năng Microsoft Defender Antivirus tích hợp trên Windows 11 bắt buộc phải được bật Real-time protection và Tamper Protection[cite: 5].
- Công cụ yêu cầu: Hệ thống cần được cài đặt sẵn bộ công cụ Microsoft Sysinternals (gồm Sysmon, Autoruns, Process Explorer), công cụ Wireshark (kèm Npcap) và môi trường Python (chỉ định phiên bản 3.14.7)[cite: 5].
- Dữ liệu bài thực hành: Cần cung cấp và giải nén gói dữ liệu `LAB3_Threats_Assets.zip` (có mã băm SHA-256 là `96236f95ce59d0cc37f9b7ab8fbb04522f21870cd0b53aad6e52da5a23655439`) vào đúng thư mục `C:\LAB3` trên máy ảo[cite: 5].