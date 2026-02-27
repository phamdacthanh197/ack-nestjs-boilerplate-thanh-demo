# Hướng dẫn cho AI Agent - Ack NestJs Boilerplate

Chào bạn! Đây là hướng dẫn dành riêng cho AI Agent để hiểu và làm việc hiệu quả với dự án này. Dự án này là một boilerplate mạnh mẽ cho NestJS sử dụng Prisma ORM và MongoDB.

## 1. Công nghệ sử dụng (Tech Stack)

- **Framework**: NestJS (v11+)
- **Ngôn ngữ**: TypeScript
- **ORM**: Prisma (v6+)
- **Cơ sở dữ liệu**: MongoDB
- **Caching & Queue**: Redis & BullMQ
- **Quản lý gói**: pnpm
- **Tài liệu API**: Swagger/OpenAPI

## 2. Cấu trúc thư mục (Project Structure)

Dự án tuân thủ cấu trúc module hóa chặt chẽ.

### Cấu trúc gốc
- `src/`: Mã nguồn chính.
  - `common/`: Chứa các thành phần dùng chung (Auth, Database, File, Helper, Pagination, Response, v.v.).
  - `modules/`: Chứa các tính năng cụ thể của ứng dụng (User, Role, Auth, Email, v.v.).
  - `router/`: Định nghĩa các routes và nhóm chúng lại (Admin, User, Public, System).
- `prisma/`: Chứa `schema.prisma` và các file liên quan đến DB.
- `docs/`: Tài liệu chi tiết về từng tính năng.
- `test/`: Các bài kiểm tra unit và integration.

### Cấu trúc trong một Module (`src/modules/<feature>/`)
Mỗi module nên có cấu trúc sau (nếu cần):
- `controllers/`: Xử lý HTTP requests.
- `services/`: Logic nghiệp vụ chính.
- `repositories/`: Tương tác với Prisma Client.
- `dtos/`: Data Transfer Objects cho Request và Response.
- `interfaces/`: Các bản thiết kế TypeScript.
- `constants/`, `enums/`, `decorators/`, `guards/`: Các thành phần hỗ trợ khác.

## 3. Quy tắc lập trình (Coding Conventions)

### Đặt tên file
- Sử dụng dấu chấm để phân tách các thành phần: `<tên-file>.<loại>.[phân-loại].ts`.
- Ví dụ: `user.service.ts`, `user.admin.controller.ts`, `user.create.request.dto.ts`.

### Lớp và Biến
- **Class**: PascalCase (ví dụ: `UserService`).
- **Biến và Hàm**: camelCase (ví dụ: `getUserById`).
- **Interface**: Bắt đầu bằng chữ `I` (ví dụ: `IUserService`).

### Luồng xử lý dữ liệu
- **Request flow**: Controller -> Service -> Repository -> Database.
- Luôn sử dụng **DTOs** để xác thực dữ liệu đầu vào bằng `class-validator`.
- Luôn sử dụng **Response Decorators** (ví dụ: `@Response`, `@ResponsePaging`) để chuẩn hóa dữ liệu trả về.

## 4. Các lệnh quan trọng (Essential Commands)

- **Cài đặt**: `pnpm install`
- **Phát triển**: `pnpm start:dev`
- **Prisma**:
  - Sinh client: `pnpm db:generate`
  - Đẩy schema lên DB: `pnpm db:migrate`
- **Kiểm tra và Định dạng**:
  - Lint: `pnpm run lint`
  - Fix lint: `pnpm run lint:fix`
  - Format: `pnpm run format`
  - Spell check: `pnpm run spell`
- **Kiểm thử**: `pnpm test`

## 5. Lưu ý cho Agent

- Trước khi thêm một module mới, hãy kiểm tra `src/common` xem có các tiện ích (helpers) hoặc decorator nào có thể tái sử dụng không.
- Khi thay đổi cơ sở dữ liệu, hãy cập nhật `prisma/schema.prisma` và chạy `pnpm db:generate`.
- Luôn kiểm tra tài liệu trong thư mục `docs/` để hiểu sâu hơn về các hệ thống cụ thể như Auth, Policy, hay File Upload.
- Tuân thủ nghiêm ngặt việc sử dụng các Alias (@app, @common, @modules) đã được định nghĩa trong `tsconfig.json`.

Chúc bạn làm việc hiệu quả!
