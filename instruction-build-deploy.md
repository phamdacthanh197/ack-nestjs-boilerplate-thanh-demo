# Hướng dẫn Build và Deploy - Ack NestJs Boilerplate

Tài liệu này cung cấp hướng dẫn về cách build ứng dụng và triển khai (deploy) lên môi trường production.

## 1. Build Ứng dụng

### 1.1. Build Local (Dùng pnpm)
Nếu bạn muốn build mã nguồn thành JavaScript thuần để chạy trên server:
```bash
# 1. Cài đặt thư viện
pnpm install --frozen-lockfile

# 2. Sinh Prisma Client
pnpm db:generate

# 3. Build mã nguồn
pnpm build
```
Kết quả build sẽ nằm trong thư mục `dist/`.

### 1.2. Build Docker Image
Dự án đã có sẵn `dockerfile` để tạo image:
```bash
docker build -t ack-nestjs-app .
```

## 2. Triển khai (Deployment)

### 2.1. Triển khai bằng Docker Compose (Khuyên dùng)
Cách nhanh nhất để triển khai toàn bộ stack (API, MongoDB, Redis, JWKS Server):

```bash
# Chạy ở chế độ background với profile 'apis'
docker-compose --profile apis up -d
```

### 2.2. Triển khai Thủ công trên Server
Nếu bạn không dùng Docker cho ứng dụng chính:

1. **Chuẩn bị môi trường**: Cài đặt Node.js v24+, Redis và MongoDB Replica Set.
2. **Biến môi trường**: Thiết lập đầy đủ các biến trong `.env` (đặc biệt là các biến Production).
3. **Cặp khóa**: Đảm bảo đã chạy `pnpm generate:keys` và lưu trữ chúng an toàn.
4. **Chạy ứng dụng**:
   ```bash
   pnpm start:prod
   ```
   *Khuyên dùng: Sử dụng PM2 để quản lý tiến trình.*

## 3. Quản lý Cơ sở dữ liệu trong Production

Trong production, bạn cần cẩn trọng khi thay đổi schema:

- **Migration**: Chạy `pnpm db:migrate` để cập nhật schema trên MongoDB.
- **Seeding**: Chỉ chạy `pnpm migration:seed` nếu cần nạp dữ liệu khởi tạo cho hệ thống mới.

## 4. Lưu ý Quan trọng cho Production

- **Bảo mật**:
  - Đảm bảo `APP_ENV=production` để tắt Swagger UI và các log debug không cần thiết.
  - Sử dụng HTTPS/SSL (có thể dùng Nginx làm Reverse Proxy).
  - Không commit file `.env` hoặc thư mục `keys/` chứa khóa bí mật lên Git.
- **Performance**:
  - Đảm bảo giới hạn tài nguyên (CPU/RAM) trong Docker hoặc PM2.
  - Monitor hệ thống qua Sentry (cấu hình `SENTRY_DSN`).
- **JWKS Server**: Dự án đi kèm một JWKS server đơn giản (Nginx) để phục vụ public keys cho việc xác thực JWT. Hãy đảm bảo cổng 3011 (hoặc cổng bạn cấu hình) được mở hoặc được proxy đúng cách.

---
*Để biết thêm chi tiết về cấu hình, hãy xem file `docker-compose.yml` và các tài liệu trong thư mục `docs/`.*
