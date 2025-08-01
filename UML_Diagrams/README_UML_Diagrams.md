# UML Diagrams cho P2P File Sharing System

## Tổng quan

Dự án này bao gồm một hệ thống chia sẻ file P2P với các biểu đồ UML sau:

## 1. Biểu đồ Lớp (Class Diagram)

**File:** `Class_Diagram.puml`

Biểu đồ này mô tả cấu trúc các lớp trong hệ thống, bao gồm:

### Backend Models
- **User**: Lớp đại diện cho người dùng với thông tin xác thực
- **FileMetadata**: Lớp lưu trữ metadata của file
- **PasswordResetToken**: Lớp quản lý token reset password
- **Role**: Enum định nghĩa vai trò người dùng

### Backend Services
- **AuthService**: Xử lý logic xác thực và đăng ký
- **FileService**: Quản lý upload/download file
- **EmailService**: Gửi email thông báo
- **JwtUtil**: Tạo và validate JWT token

### Backend Controllers
- **AuthController**: API endpoints cho xác thực
- **FileController**: API endpoints cho quản lý file

### Frontend Components
- **App**: Component chính quản lý routing
- **Login/Register**: Components xác thực
- **Dashboard/Home**: Components giao diện chính

### DTOs
- **LoginRequest/RegisterRequest**: Request objects
- **AuthResponse/FileUploadResponse**: Response objects
- **ApiResponse**: Generic response wrapper

## 2. Biểu đồ Hoạt động (Activity Diagrams)

### 2.1. Đăng nhập (Login)
**File:** `Activity_Login.puml`

Mô tả quy trình đăng nhập:
1. User nhập email/password
2. Frontend gửi request
3. Backend xác thực
4. Tạo JWT token
5. Trả về response và chuyển hướng

### 2.2. Đăng ký (Register)
**File:** `Activity_Register.puml`

Mô tả quy trình đăng ký:
1. User nhập thông tin
2. Validate dữ liệu
3. Kiểm tra email tồn tại
4. Hash password và tạo user
5. Gửi email chào mừng

### 2.3. Upload File
**File:** `Activity_FileUpload.puml`

Mô tả quy trình upload file:
1. User chọn file
2. Validate file
3. Lưu vào GridFS
4. Tạo metadata
5. Trả về response

### 2.4. Quên mật khẩu (Forgot Password)
**File:** `Activity_ForgotPassword.puml`

Mô tả quy trình quên mật khẩu:
1. User nhập email
2. Tạo reset token
3. Gửi email reset
4. Bảo mật thông tin

### 2.5. Đặt lại mật khẩu (Reset Password)
**File:** `Activity_ResetPassword.puml`

Mô tả quy trình reset password:
1. Validate token
2. Kiểm tra hạn sử dụng
3. Cập nhật password
4. Đánh dấu token đã sử dụng

## 3. Biểu đồ Sequence

### 3.1. Authentication Sequence
**File:** `Sequence_Auth.puml`

Mô tả tương tác giữa các component trong quá trình xác thực:
- Login process
- Register process
- Forgot password process
- Reset password process

### 3.2. File Management Sequence
**File:** `Sequence_FileManagement.puml`

Mô tả tương tác trong quản lý file:
- Upload file process
- Download file process
- Get user files process
- Delete file process
- Rename file process

## 4. Biểu đồ Use Case

### 4.1. Use Case Tổng quan
**File:** `UseCase_General.puml`

Mô tả các use case chính của hệ thống:
- **User**: Đăng ký, đăng nhập, quản lý file
- **Admin**: Quản lý người dùng, thống kê hệ thống

## Cách sử dụng

### Xem biểu đồ online
1. Copy nội dung file `.puml`
2. Paste vào [PlantUML Online Server](http://www.plantuml.com/plantuml/uml/)
3. Xem kết quả

### Generate hình ảnh
```bash
# Cài đặt PlantUML
npm install -g plantuml

# Generate PNG
plantuml Class_Diagram.puml

# Generate SVG
plantuml -tsvg Class_Diagram.puml
```

## Cấu trúc hệ thống

### Backend (Spring Boot)
- **Framework**: Spring Boot 3.x
- **Database**: MongoDB
- **File Storage**: GridFS
- **Security**: JWT + Spring Security
- **Email**: JavaMailSender

### Frontend (React + TypeScript)
- **Framework**: React 18
- **Language**: TypeScript
- **Build Tool**: Vite
- **Routing**: React Router
- **State Management**: React Hooks

### Mobile (React Native)
- **Framework**: React Native + Expo
- **Language**: JavaScript
- **Platform**: Cross-platform (iOS/Android)

## Tính năng chính

1. **Authentication**
   - Đăng ký/Đăng nhập
   - JWT token authentication
   - Forgot/Reset password
   - Email verification

2. **File Management**
   - Upload/Download files
   - File metadata management
   - File type classification
   - File sharing

3. **Security**
   - Password hashing (BCrypt)
   - JWT token validation
   - File access control
   - CORS configuration

4. **User Experience**
   - Responsive design
   - Real-time feedback
   - Error handling
   - Loading states

## Database Schema

### Users Collection
```javascript
{
  _id: ObjectId,
  email: String (unique),
  password: String (hashed),
  firstName: String,
  lastName: String,
  roles: Array,
  enabled: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

### File Metadata Collection
```javascript
{
  _id: ObjectId,
  fileName: String,
  originalFileName: String,
  contentType: String,
  fileSize: Number,
  gridFsId: String,
  uploadedBy: String,
  uploadedAt: Date,
  fileType: String
}
```

### Password Reset Tokens Collection
```javascript
{
  _id: ObjectId,
  token: String (unique),
  email: String,
  createdAt: Date,
  expiresAt: Date,
  used: Boolean
}
``` 