# BTVN4_Wordpress-N8n
## Tạo file docker-compose.yml với nội dung

`version: "3.8"

services:
  mariadb:
    image: mariadb:latest
    container_name: mariadb
    restart: unless-stopped
    environment:
      TZ: "Asia/Ho_Chi_Minh"
      MARIADB_ROOT_PASSWORD: rootpass123
      MARIADB_DATABASE: wordpress_db
      MARIADB_USER: wp_user
      MARIADB_PASSWORD: wp_pass123
    volumes:
      - mariadb_data:/var/lib/mysql
    networks:
      - stack_net

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: phpmyadmin
    restart: unless-stopped
    environment:
      PMA_HOST: mariadb
      PMA_ARBITRARY: 1
    depends_on:
      - mariadb
    networks:
      - stack_net

  wordpress:
    image: wordpress:latest
    container_name: wordpress
    restart: unless-stopped
    environment:
      WORDPRESS_DB_HOST: mariadb
      WORDPRESS_DB_NAME: wordpress_db
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: wp_pass123
    volumes:
      - wordpress_data:/var/www/html
    depends_on:
      - mariadb
    networks:
      - stack_net

  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    restart: unless-stopped
    command: tunnel --no-autoupdate run --token eyJhIjoiYWJhNWU1NmNjOGZhZmNlYzhhNzZkZjJkNzJmM2UxOWQiLCJ0IjoiNjgxN2FhMjktZDE4Mi00MDQ4LTljMTktNTBhNWQxYTJiMzUzIiwicyI6IlkyRTJaVGxsTURZdFpqRTFZeTAwT0dVM0xXSXpOMk10WVRZM1kyRTNabVl4T1ROaE1UQTBNRGhrT0RZdE9XWTJZUzAwTmpSbUxUa3dZamN0TXpkaU1qTTRaRFk1TVRaaiJ9
    networks:
      - stack_net

  n8n:
    image: n8nio/n8n:latest
    container_name: n8n
    restart: unless-stopped
    environment:
      WEBHOOK_URL: https://k58-n8n.duongly204.io.vn/
      N8N_HOST: 0.0.0.0
      N8N_PORT: 5678
    volumes:
      - n8n_data:/home/node/.n8n
    networks:
      - stack_net

volumes:
  mariadb_data:
  wordpress_data:
  n8n_data:

networks:
  stack_net:
    driver: bridge`


<img width="1287" height="211" alt="image" src="https://github.com/user-attachments/assets/63a2a44c-f40b-4acb-99c7-09b4f5ed58c8" />

## Cấu hình Cloudflare Tunnel (Add Routes)

Trong dashboard Cloudflare → Networks → Tunnels → chọn tunnel vừa tạo → Public Hostname → Add a public hostname

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f5c9adff-1a2b-4cf0-b800-b4871fefcb42" />

## Lấy ip dán vào taget cho trỏ đúng k58-tunnel

<img width="1149" height="85" alt="image" src="https://github.com/user-attachments/assets/05e1cfd9-bc71-4774-a0c5-dc12041511e6" />

<img width="1536" height="354" alt="image" src="https://github.com/user-attachments/assets/9ad98789-b98f-4b51-aeed-07759f242142" />

## Kiểm tra phpMyAdmin và cài WordPress

- Kiểm tra DB trống: Truy cập https://k58-pma.tdh.io.vn → đăng nhập user wp_user / wp_pass123 → chọn database wordpress_db → quan sát chưa có bảng nào.
- 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c2f94520-e571-4bd3-b2d0-ad0c7c0eec6e" />

- Cài WordPress: Truy cập https://k58-wp.duongly204.io.vn → làm theo wizard (chọn ngôn ngữ → điền tên site, admin, email, mật khẩu → Install WordPress).
- 
  <img width="762" height="884" alt="image" src="https://github.com/user-attachments/assets/bfbb1c8e-9fa8-4109-b591-be9eea6350e0" />

-Sau khi cài xong: Quay lại phpMyAdmin → reload → thấy các bảng wp_posts, wp_users, wp_options, ... (khoảng 12 bảng).

<img width="1906" height="1050" alt="image" src="https://github.com/user-attachments/assets/57b89030-717a-4d05-844a-6e3f4a4909ef" />

## Cấu hình N8N

Tạo tài khoản admin: Điền tên, email, mật khẩu → Next
Lấy License key: Chọn "Send me a license key" → điền thông tin → Submit → Kiểm tra email lấy key
Kích hoạt: Settings (góc dưới trái) → Usage and plan → Enter activation key → Activate

## Tạo Telegram Bot

Tạo not mới:
<img width="1125" height="2436" alt="image" src="https://github.com/user-attachments/assets/b72e8cf2-da6d-4ccd-aad1-022ad4d453ad" />

## truy cập https://aistudio.google.com/api-keys để lấy key

<img width="1915" height="1069" alt="image" src="https://github.com/user-attachments/assets/273da029-36f2-4568-a8a7-acbd8720b41f" />

## thiết lập các node gemini,telegram,javascript

<img width="1696" height="929" alt="image" src="https://github.com/user-attachments/assets/12b65f53-9485-4a1c-a3a1-e20521bc57bd" />

<img width="1902" height="1028" alt="image" src="https://github.com/user-attachments/assets/68efa72c-0e92-4376-87de-05c0914e8c51" />

<img width="1867" height="840" alt="image" src="https://github.com/user-attachments/assets/86f5dd05-7df6-418c-8c45-d306f07a5945" />

## Cấu hình và test tất cả successfully 

<img width="1426" height="516" alt="image" src="https://github.com/user-attachments/assets/9ff4c1cc-714f-403d-bf5b-4173ec5aa690" />

## Thành công post bài tự động

<img width="1125" height="2436" alt="image" src="https://github.com/user-attachments/assets/6217d6f4-7be7-4c60-9ea4-189ed644a4e6" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0b261647-2232-4ce9-82e2-4e5025e4df0d" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/274225b6-6f97-4a77-ad6a-e2cd2a32ffd6" />


