# LAB 9 – Local DNS Attack: DNS Spoofing và Cache Poisoning

**Họ và tên:** Trần Phạm Thiên Ân  
**MSSV:** 1150080127  
**Tên bài LAB:** Lab 9 - Local DNS Attack: DNS Spoofing và Cache Poisoning trong mạng ảo cô lập

## Nội dung thực hiện

- Thiết lập phân đoạn mạng ảo cô lập DNS-LAB (\`10.9.0.0/24\`) trên pfSense, cấu hình các firewall rule bảo vệ hạ tầng nội bộ và kiểm soát lưu lượng ra ngoài.
- Cấu hình DNS-SERVER sử dụng BIND9 trên Ubuntu Server, gồm zone \`lab9.test\`, reverse zone cho dải \`10.9.0.0/24\` và forward zone \`external.lab9.test\` tới AUTH-DNS mô phỏng tại DMZ.
- Cấu hình VICTIM/Client-A sử dụng DNS duy nhất \`10.9.0.10\` và đo kiểm kết quả phân giải đường cơ sở trước khi tấn công.
- Khởi động AUTH-DNS mô phỏng trên DMZ-WEB/APP-TEST tại \`172.16.6.20\`, trả lời \`www.external.lab9.test\` về máy web thật \`172.16.6.10\` với độ trễ 1,5 giây.
- **Task 1 – HOSTS file:** chuyển hướng cục bộ tên miền về IP Kali và đối chiếu kết quả của trình duyệt/curl với truy vấn \`dig\` trực tiếp tới DNS-SERVER.
- **Task 2 – Direct DNS Spoofing:** dùng Kali và Scapy bắt truy vấn DNS của VICTIM, sau đó gửi phản hồi giả trực tiếp về VICTIM với Transaction ID và UDP port tương ứng.
- **Task 3 – DNS Cache Poisoning:** gửi phản hồi giả tới DNS-SERVER để BIND9 lưu bản ghi độc hại vào cache, từ đó ảnh hưởng các client dùng resolver này.
- Khôi phục môi trường bằng cách dừng script spoof, xóa cache DNS, khôi phục hosts file và kiểm tra lại kết quả hợp lệ.

## Kết quả thực hiện

- pfSense định tuyến đúng mạng DNS-LAB; các rule cô lập ngăn lưu lượng không được phép tới các mạng nhạy cảm và WAN.
- BIND9 kiểm tra cấu hình/zone thành công; baseline của \`www.external.lab9.test\` trả về IP hợp lệ \`172.16.6.10\`.
- **Task 1:** ứng dụng trên VICTIM bị chuyển hướng về Fake Site tại \`10.9.0.200\`, trong khi \`dig @10.9.0.10\` vẫn trả IP gốc vì \`dig\` không sử dụng hosts file.
- **Task 2:** Kali ghi nhận các forged victim response; VICTIM truy cập domain và nhận trang mô phỏng do Kali phục vụ.
- **Task 3:** cache dump của BIND9 ghi nhận \`www.external.lab9.test\` trỏ về \`10.9.0.200\` với TTL còn lại; các client truy vấn resolver có thể nhận kết quả đã bị đầu độc.
- Sau khi dừng spoof và flush cache, kết quả phân giải trở lại \`172.16.6.10\`.


## Lưu ý để giảng viên kiểm tra hoặc chạy lại

- Cần có các máy ảo pfSense, DNS-SERVER, VICTIM/Client-A, Kali, DMZ-WEB và AUTH-DNS/APP-TEST.
- Interface DNSLAB trên pfSense dùng \`10.9.0.1/24\` và không cấu hình upstream gateway.
- Các máy trong DNS-LAB phải nối cùng mạng Host Only/Internal Network; trong UTM, nếu cần quan sát traffic unicast, phải dùng cấu hình on-path/neighbor tĩnh theo hướng dẫn của báo cáo.
- AUTH-DNS phải có địa chỉ phụ \`172.16.6.20/24\` và chạy script \`auth_dns_delayed.py\` trên UDP/53.
- DNS-SERVER forward \`external.lab9.test\` tới \`172.16.6.20\`; bản ghi thật được AUTH-DNS trả về là \`172.16.6.10\`.
- Kali dùng địa chỉ \`10.9.0.200\` và chạy \`dns_spoof_lab9.py\` trên đúng interface DNS-LAB.
- Khi chạy lại từng task, xóa cache cũ trước bằng \`sudo rndc flush\` trên DNS-SERVER và \`ipconfig /flushdns\` trên Windows.
- Sau mỗi kịch bản, dừng tiến trình spoof bằng \`Ctrl+C\`, khôi phục hosts file, xóa neighbor/on-path tạm thời và kiểm tra lại baseline.
- Toàn bộ thao tác chỉ thực hiện trong mạng ảo cô lập phục vụ học tập; không áp dụng trên mạng hoặc hệ thống không được phép.


