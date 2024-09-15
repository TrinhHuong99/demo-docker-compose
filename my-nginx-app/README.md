services:
  web:
    image: nginx
    ports:
      - "8080:80"
    networks:
      - nginx-app
networks:
  nginx-app:
    driver: bridge #tùy chọn kiểu mạng
    name: nginx-app # Đặt tên mạng cố định


------------------
Khi sử dụng mạng host, bạn không cần khai báo ports: trong Docker Compose vì container sử dụng cổng của máy chủ trực tiếp.
Ví dụ, Nginx lắng nghe cổng 80 bên trong container sẽ lắng nghe trực tiếp cổng 80 của máy chủ khi sử dụng mạng host. Bạn có thể kiểm tra cấu hình của Nginx (hoặc ứng dụng web bạn đang chạy) để biết container lắng nghe trên cổng nào.
services:
  web:
    image: nginx
    network_mode: host # Sử dụng trực tiếp mạng host

----------------------
- network
Bên trong service: Kết nối service cụ thể với mạng.
Bên ngoài service: Định nghĩa và tạo mạng để Docker Compose sử dụng.
- type driver :
. Bridge Network (Cầu nối) - driver: bridge (Kiểu mặc định):
Mặc định khi không chỉ định kiểu mạng nào khác. Mỗi container được kết nối với một mạng ảo riêng.
Containers trong mạng bridge có thể giao tiếp với nhau qua địa chỉ IP hoặc tên service.
Containers không tự động có thể giao tiếp với máy chủ host (ngoài trừ khi bạn ánh xạ cổng như trong ví dụ 8080:80).
2. Host Network - driver: host:
Sử dụng mạng của máy chủ host. Containers sẽ chia sẻ mạng trực tiếp với máy chủ host, có nghĩa là không có sự tách biệt về địa chỉ IP hay cổng mạng.
Containers sử dụng mạng host sẽ chia sẻ tất cả các cổng mạng với máy chủ host. Không cần phải ánh xạ cổng (như 8080:80) vì container sẽ dùng cổng của host trực tiếp.
Sử dụng khi: Bạn cần tối ưu hiệu suất mạng hoặc cần trực tiếp truy cập các tài nguyên mạng của máy chủ.
Ví dụ sử dụng driver: host:

yaml
Sao chép mã
networks:
  nginx-app:
    driver: host
    name: nginx-app
3. None Network (Không có mạng) - driver: null:
Không có mạng nào được gán. Containers sẽ hoàn toàn không có mạng, không thể giao tiếp với các container khác hay mạng ngoài.
Sử dụng khi: Container không cần mạng, chẳng hạn như khi bạn chỉ chạy các tác vụ không liên quan đến mạng, như xử lý dữ liệu hoặc công việc batch.
Chú ý:
Với driver: host, không cần ánh xạ cổng trong phần ports: vì container sẽ chia sẻ toàn bộ cổng mạng với máy chủ.
Với driver: null, container sẽ không có bất kỳ kết nối mạng nào, không thể truy cập được từ bên ngoài.
Tùy thuộc vào yêu cầu của dự án, bạn có thể chọn kiểu mạng phù hợp.
