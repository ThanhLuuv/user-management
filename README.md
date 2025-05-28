# Laravel + React + MySQL Project

## Mô tả dự án

Dự án full-stack được xây dựng với:
- **Backend**: Laravel (PHP 8.x+)
- **Frontend**: React 
- **Database**: MySQL 
- **Reverse Proxy**: Nginx

## Yêu cầu hệ thống

### Chạy với Docker (khuyến nghị)
- Docker >= 20.x
- Docker Compose >= 1.27.x

### Chạy thủ công
- PHP >= 8.x
- Composer
- Node.js >= 16.x
- npm hoặc yarn
- MySQL >= 5.7

## Cài đặt và chạy dự án

### Phương pháp 1: Sử dụng Docker Compose (Khuyến nghị)

#### 1. Clone repository và di chuyển vào thư mục dự án
```bash
git clone --recurse-submodules https://github.com/ThanhLuuv/user-management.git
cd user-management
```

#### 2. Di chuyển vào thư mục backend và copy file .env.example thành file .env
```bash
cd user-management-backend
cp .env.example .env
```

#### 3. Trở lại thư mục dự án để Build và khởi chạy tất cả container
```bash
cd ..
docker-compose up --build -d
```
#### 4. Truy cập ứng dụng
- **Frontend**: http://localhost/
- **Backend API**: http://localhost/backend
- **Database**: localhost:3306

### Phương pháp 2: Chạy thủ công

#### 1. Chuẩn bị Database
```bash
# Tạo database MySQL
mysql -u root -p
CREATE DATABASE laravel_app;
CREATE USER 'laravel_user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON laravel_app.* TO 'laravel_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

#### 2. Cài đặt Backend (Laravel)
```bash
cd backend

# Cài đặt dependencies
composer install

# Tạo file môi trường
cp .env.example .env

# Tạo application key
php artisan key:generate

# Tạo jwt:secret
php artisan jwt:secret

# Cấu hình database trong .env
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:xxxxx
APP_DEBUG=true
LOG_LEVEL=debug
APP_URL=http://localhost
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel_db_user
DB_USERNAME=laravel_user
DB_PASSWORD=laravel_pass

# Chạy migration
php artisan migrate

# (Tùy chọn) Seed dữ liệu
php artisan db:seed

# Tạo secret cho jwt
php artisan jwt:secret

# Khởi chạy server
php artisan serve
```

Backend sẽ chạy trên: http://localhost:8000

#### 3. Cài đặt Frontend (React)
```bash
cd frontend

# Cài đặt dependencies
npm install

# Cấu hình API endpoint (tạo file .env nếu cần)
# REACT_APP_API_URL=http://localhost:8000/api

# Khởi chạy development server
npm start
```

Frontend sẽ chạy trên: http://localhost:3000

## Các lệnh Docker hữu ích

```bash
# Dừng tất cả container
docker-compose down

# Dừng và xóa volumes (cẩn thận - sẽ xóa dữ liệu database)
docker-compose down -v

# Xem logs của tất cả services
docker-compose logs -f

# Xem logs của service cụ thể
docker-compose logs -f backend
docker-compose logs -f frontend
docker-compose logs -f mysql

# Truy cập shell của container
docker-compose exec backend bash
docker-compose exec frontend sh

# Rebuild container khi có thay đổi
docker-compose up --build

# Chạy lệnh artisan
docker-compose exec backend php artisan <command>

# Chạy composer
docker-compose exec backend composer <command>

# Chạy npm
docker-compose exec frontend npm <command>
```

## Cấu hình

### Environment Variables

#### Backend (.env)
```env
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:...
APP_DEBUG=true
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=mysql  # 'mysql' nếu dùng Docker, '127.0.0.1' nếu local
DB_PORT=3306
DB_DATABASE=laravel_app
DB_USERNAME=laravel_user
DB_PASSWORD=password
```

#### Frontend (.env)
```env
REACT_APP_API_URL=http://localhost/backend # Docker
# REACT_APP_API_URL=http://localhost:8000/  # Local
```

## Testing

```bash
# Backend tests
docker-compose exec backend php artisan test
# hoặc local: php artisan test

# Frontend tests  
docker-compose exec frontend npm test
# hoặc local: npm test
```

## API Documentation

API endpoints có thể được truy cập tại:
- Base URL: `http://localhost/backend` (Docker) hoặc `http://localhost:8000/api` (Local)

## Troubleshooting

### Container không khởi động được
```bash
# Kiểm tra logs
docker-compose logs

# Kiểm tra trạng thái container
docker-compose ps

# Rebuild từ đầu
docker-compose down
docker-compose up --build
```

### Lỗi database connection
- Kiểm tra cấu hình database trong `.env`
- Đảm bảo MySQL container đã sẵn sàng trước khi chạy migration
- Kiểm tra port 3306 không bị conflict

### Frontend không gọi được API
- Kiểm tra `REACT_APP_API_URL` trong `.env`
- Kiểm tra CORS configuration trong Laravel
- Kiểm tra network connection giữa containers


## Hỗ trợ

Nếu gặp vấn đề, vui lòng:
- Tạo issue trên GitHub
- Liên hệ team phát triển
- Kiểm tra documentation

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Phiên bản**: 1.0.0  
**Cập nhật lần cuối**: 28/05/2025
**Tác giả**: ThanhLuuv