# Hướng dẫn Chạy Dự án - Ack NestJs Boilerplate

Tài liệu này hướng dẫn chi tiết các bước để thiết lập và chạy dự án Ack NestJs Boilerplate trên môi trường local.

## 1. Yêu cầu Tiên quyết

Trước khi bắt đầu, hãy đảm bảo máy tính của bạn đã cài đặt các công cụ sau:

- **Node.js**: Phiên bản >= 24.11.0
- **pnpm**: Phiên bản >= 10.25.0
- **MongoDB**: Phải chạy ở chế độ **Replica Set** (để hỗ trợ database transactions).
- **Redis**: Dùng cho caching và BullMQ.
- **Docker & Docker Compose** (Tùy chọn nhưng khuyến khích): Để khởi chạy nhanh các dịch vụ phụ trợ.

## 2. Các bước Thiết lập

### Bước 1: Clone dự án và Cài đặt thư viện
```bash
# Cài đặt các phụ thuộc
pnpm install
```

### Bước 2: Thiết lập biến môi trường
```bash
# Tạo file .env từ file ví dụ
cp .env.example .env
```
*Lưu ý: Chỉnh sửa file `.env` để phù hợp với cấu hình local của bạn (đặc biệt là `DATABASE_URL` và `CACHE_REDIS_URL`).*

### Bước 3: Khởi chạy các dịch vụ phụ trợ (Dùng Docker)
Nếu bạn đã cài đặt Docker, bạn có thể khởi chạy MongoDB, Redis và JWKS server bằng lệnh:
```bash
docker-compose up -d
```

### Bước 4: Tạo cặp khóa bảo mật (Keys)
Dự án sử dụng cặp khóa Public/Private để ký JWT.
```bash
pnpm generate:keys
```

### Bước 5: Thiết lập Cơ sở dữ liệu
```bash
# Sinh Prisma Client
pnpm db:generate

# Đẩy cấu hình schema lên MongoDB
pnpm db:migrate

# Nạp dữ liệu mẫu (Seeding)
pnpm migration:seed
```

## 3. Chạy Ứng dụng

### Chế độ Phát triển (Development)
```bash
pnpm start:dev
```
Ứng dụng sẽ chạy tại: `http://localhost:3000`

### Chế độ Sản xuất (Production)
```bash
# Build dự án
pnpm build

# Chạy bản build
pnpm start:prod
```

## 4. Kiểm tra và Tài liệu API

- **Swagger UI**: `http://localhost:3000/docs` (Chỉ khả dụng khi `APP_ENV` không phải là `production`).
- **Kiểm tra API đơn giản**: `http://localhost:3000/api/hello`
- **Queue Dashboard** (Nếu dùng Docker): `http://localhost:3010` (User: `admin`, Pass: `admin123`).

## 5. Các lệnh hữu ích khác

- **Kiểm tra lỗi mã nguồn**: `pnpm run lint`
- **Sửa lỗi format tự động**: `pnpm run format`
- **Chạy Tests**: `pnpm test`
- **Kiểm tra code thừa**: `pnpm deadcode`

---
*Nếu gặp khó khăn, hãy tham khảo thêm tài liệu chi tiết trong thư mục `docs/`.*
