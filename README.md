Bài 4: Cấu hình tường lửa bảo vệ máy chủ (UFW \& Infrastructure Firewall)



1\. Mục tiêu



Thiết lập tường lửa UFW trên hệ điều hành Ubuntu.



Chặn mặc định các kết nối đi vào.



Cho phép các kết nối đi ra.



Chỉ cho phép lưu lượng SSH qua cổng 22/tcp và HTTP qua cổng 80/tcp.



Kết hợp với Infrastructure Firewall của WiServices để bảo vệ máy chủ

từ tầng hạ tầng.



2\. Thông tin máy chủ



Hệ điều hành: Ubuntu 24.10



IP máy chủ: 221.121.3.147



SSH: TCP port 22



HTTP: TCP port 80



3\. Cấu hình UFW



Thiết lập chính sách mặc định:



sudo ufw default deny incoming

sudo ufw default allow outgoing



Cho phép SSH:



sudo ufw allow 22/tcp



Cho phép HTTP:



sudo ufw allow 80/tcp



Kích hoạt UFW:



sudo ufw enable



4\. Kiểm tra UFW



Sử dụng lệnh:



sudo ufw status verbose



Kết quả:



Status: active

Logging: on (low)

Default: deny (incoming), allow (outgoing), disabled (routed)



To                         Action      From

\--                         ------      ----

80/tcp                     ALLOW IN    Anywhere

22/tcp                     ALLOW IN    Anywhere

80/tcp (v6)                ALLOW IN    Anywhere (v6)

22/tcp (v6)                ALLOW IN    Anywhere (v6)







Cấu hình trên cho thấy:



UFW đang ở trạng thái active.



Mặc định các kết nối đi vào bị từ chối (deny incoming).



Các kết nối đi ra được cho phép (allow outgoing).



Chỉ cho phép SSH trên port 22 và HTTP trên port 80.



5\. Cấu hình Infrastructure Firewall



Máy chủ được triển khai trên WiServices nên Firewall ở tầng hạ tầng được

cấu hình trên WiServices.



Các inbound rule được cấu hình:



Direction   Protocol   Source          Destination Port Action   Service



IN          TCP        0.0.0.0/0                   80 ACCEPT   HTTP

IN          SSH        Any                           22 ACCEPT   SSH



Ảnh cấu hình Infrastructure Firewall:







Firewall tầng hạ tầng chỉ cho phép các dịch vụ cần thiết là HTTP và SSH.



6\. Kiểm tra kết nối SSH



Từ máy tính cá nhân, sử dụng PowerShell:



ssh root@221.121.3.147



Kết quả đăng nhập thành công:



Welcome to Ubuntu 24.10

root@devops-ubuntu:\~#







Điều này chứng minh port 22/tcp cho phép kết nối SSH từ máy cá nhân.



7\. Kiểm tra kết nối HTTP



Truy cập website bằng trình duyệt:



http://221.121.3.147



Website hiển thị:



Welcome to PTIT DevOps Course - Session 02



Ảnh kết quả truy cập HTTP:







Điều này chứng minh port 80/tcp cho phép kết nối HTTP.



8\. Kiểm tra port không được mở



Sử dụng PowerShell trên máy cá nhân để kiểm tra port 8080:



Test-NetConnection 221.121.3.147 -Port 8080



Kết quả:



RemotePort        : 8080

TcpTestSucceeded  : False







Kết quả TcpTestSucceeded : False cho thấy kết nối TCP đến port 8080

không thành công. Port 8080 không được cấu hình cho phép trong UFW và

Infrastructure Firewall.



9\. Tổng kết cấu hình



Thành phần                Cấu hình



UFW                       Active

Incoming mặc định         Deny

Outgoing mặc định         Allow

SSH                       Port 22/tcp - Allow

HTTP                      Port 80/tcp - Allow

Port 8080                 Không cho phép

Infrastructure Firewall   Cho phép SSH và HTTP



10\. Kết luận



Máy chủ đã được cấu hình tường lửa với hai lớp bảo vệ:



UFW ở tầng hệ điều hành Ubuntu.



Infrastructure Firewall ở tầng hạ tầng WiServices.



Cấu hình chỉ cho phép các dịch vụ cần thiết gồm SSH trên port 22 và HTTP

trên port 80, đồng thời chặn các kết nối đến các port dịch vụ không được

sử dụng.

