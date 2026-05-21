# NGuyenTuanAnh_BT4_PTUDVMNM
# Môn: Phát triển ứng dụng với mã nguồn mở-TEE0421
## Lớp: 58KTPM
## Bài tập 04:
# KHAI THÁC N8N ĐỂ TỰ ĐỘNG ĐĂNG BÀI LÊN WORDPRESS 
## SỬ DỤNG KẾT QUẢ ĐÃ LÀM Ở BÀI TẬP 3, BỔ SUNG VÀO DOCKER COMPOSE ĐỂ CÓ THÊM SERVICE 8N8:

1. SỬ DỤNG DOCKER TRÊN UBUNTU ĐỂ TẠO 1 file docker-compose.yml chứa:
   - Mariadb: sử dụng image: mariadb:latest để làm hệ quản trị csdl cho wordpress, thêm các biến môi trường: TZ: "Asia/Ho_Chi_Minh", MARIADB_ROOT_PASSWORD, MARIADB_DATABASE, MARIADB_USER, MARIADB_PASSWORD (giá trị tuỳ ý)
   - Phpmyadmin: sử dụng image: phpmyadmin:latest để đăng nhập vào mariadb rồi tạo csdl trống (chỉ để xem, ko cần tạo bảng từ đây, wordpress sẽ làm hết), khai báo biến môi trường: PMA_HOST: <tên service mariadb>, PMA_ARBITRARY: 1
   - WordPress: sử dụng image: wordpress:latest, truyền các tham số môi trường cho wordpress là các thông tin truy cập csdl mariadb, tạo bởi Phpmyadmin, khai báo biến môi trường: WORDPRESS_DB_HOST: <tên service mariadb>, WORDPRESS_DB_NAME, WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD (giá trị theo mariadb đã khai báo)
   - Cloudflared: sử dụng image: cloudflare/cloudflared:latest , full command và token lấy từ dashboard của cloudflare, dùng AI chuyển sang dạng docker compose
N8n : sử dụng image: n8nio/n8n:latest, nhớ truyền biến môi trường WEBHOOK_URL theo sub-domain đã add router cho cloudflared tunnel

<img width="961" height="541" alt="image" src="https://github.com/user-attachments/assets/f65c5200-1e9b-44c1-8bc5-842b1715c600" />
<img width="966" height="544" alt="image" src="https://github.com/user-attachments/assets/c21d6f8a-5c38-4374-90c1-dbec5a9ccdd0" />

Truy cập sub-domain2 để quan sát xem cơ sở dữ liệu chưa có bảng nào:
- Container mariadb và phpmyadmin đã khởi chạy thành công, kết nối được với nhau thông qua mạng nội bộ của Docker (Docker Network).
  
-Việc phân quyền user và khởi tạo database ban đầu hoàn toàn chính xác.

<img width="1635" height="866" alt="image" src="https://github.com/user-attachments/assets/822206e4-d857-4607-829a-6fca3d9826f0" />






