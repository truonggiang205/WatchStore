# WatchStore Backend

Backend cho website bán đồng hồ WatchStore, xây dựng bằng Spring Boot. Hệ thống cung cấp API cho frontend quản lý sản phẩm đồng hồ, giỏ hàng, đơn hàng, đánh giá, voucher, bảo hành, nhập hàng, nhà cung cấp, thông báo, chat hỗ trợ và báo cáo.

## Tính năng chính

- Xác thực và phân quyền bằng JWT cho các vai trò `CUSTOMER`, `STAFF`, `OWNER`.
- Đăng ký tài khoản khách hàng với OTP email, đăng nhập bằng username hoặc email, quên mật khẩu và đổi mật khẩu.
- Quản lý người dùng, khách hàng, nhân viên và khóa/mở khóa tài khoản.
- Quản lý danh mục, sản phẩm đồng hồ, ảnh sản phẩm và so sánh sản phẩm.
- Giỏ hàng, đặt hàng, QR payment, hủy đơn, cập nhật trạng thái đơn hàng và lịch sử trạng thái.
- Đánh giá sản phẩm, voucher, nhà cung cấp và phiếu nhập hàng.
- Yêu cầu bảo hành và quy trình duyệt/từ chối bảo hành.
- Thông báo hệ thống, chat AI, chat hỗ trợ giữa khách hàng và nhân viên.
- Báo cáo dashboard, doanh thu, đơn hàng theo ngày/tháng và sản phẩm bán chạy.

## Công nghệ

- Java 17
- Spring Boot 3.5
- Spring Web
- Spring Security
- Spring Data JPA
- MySQL
- Maven Wrapper
- Lombok
- JJWT
- Cloudinary
- Spring Mail
- Spring AI Google GenAI
- Qdrant vector store

## Cấu trúc thư mục

```text
demo/
  src/main/java/com/example/demo/
    config/          # cấu hình security, CORS, JWT, AI, exception
    controllers/     # REST API controllers
    dtos/            # request/response DTO
    entities/        # JPA entities và enum
    exceptions/      # exception riêng
    repositories/    # Spring Data repositories
    services/        # business logic
  docs/              # tài liệu kỹ thuật và data SQL
  API_FLOW.md        # tổng hợp API và flow nghiệp vụ
```

## Yêu cầu môi trường

- Java 17
- MySQL 8 hoặc tương đương
- Maven Wrapper đã có sẵn trong repo
- Tài khoản/dịch vụ tùy chọn nếu dùng đầy đủ tính năng: Cloudinary, SMTP mail, Google GenAI, Qdrant

## Cài đặt và chạy local

```bash
cd demo
./mvnw spring-boot:run
```

Trên Windows PowerShell:

```powershell
cd demo
.\mvnw.cmd spring-boot:run
```

API mặc định chạy tại:

```text
http://localhost:8080/api
```

## Cấu hình ứng dụng

Repo hiện chưa có file `src/main/resources/application.properties`, vì vậy cần tạo file này khi chạy local. Ví dụ cấu hình tối thiểu:

```properties
spring.application.name=WatchStore

spring.datasource.url=jdbc:mysql://localhost:3306/watchstore?createDatabaseIfNotExist=true&useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect

jwt.secret=replace_this_with_a_long_secure_secret_at_least_32_chars
jwt.expiration-ms=86400000

app.cors.allowed-origin-patterns=http://localhost:3000,http://localhost:5173
```

Cấu hình tùy chọn cho các tính năng nâng cao:

```properties
# Email OTP và email thông báo
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=your_email@gmail.com
spring.mail.password=your_app_password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
app.mail.from=${spring.mail.username}

# Cloudinary upload ảnh sản phẩm
cloudinary.url=cloudinary://api_key:api_secret@cloud_name
cloudinary.folder=project-cnpm/products

# QR payment
app.qr.account-number=0359537981
app.qr.bank-code=MB
app.rio.poll-url=https://thanhdat050625.onrender.com/api/rio/cnpm/sent
app.rio.timeout-ms=8000

# Notification bot
notification.system-bot.username=system.bot
notification.system-bot.email=system.bot@local
notification.system-bot.password=system-bot-secret

# AI / Qdrant
spring.ai.google.genai.api-key=your_google_genai_key
spring.ai.vectorstore.qdrant.host=localhost
spring.ai.vectorstore.qdrant.port=6334
app.ai.qdrant.search-top-k=5
```

## Database

Ứng dụng dùng Spring Data JPA với MySQL. Có thể để Hibernate tự tạo/cập nhật schema bằng:

```properties
spring.jpa.hibernate.ddl-auto=update
```

Nếu cần dữ liệu mẫu hoặc tham khảo cấu trúc dữ liệu, xem:

- `demo/docs/data_v2.sql`
- `data`

## API chính

Base path: `/api`

- `/api/auth`: đăng ký, xác thực email, đăng nhập, quên mật khẩu, reset mật khẩu.
- `/api/users`, `/api/customers`, `/api/staff`: quản lý tài khoản và người dùng.
- `/api/categories`, `/api/products`: danh mục, sản phẩm, ảnh sản phẩm và so sánh.
- `/api/cart`, `/api/orders`: giỏ hàng, đặt hàng, QR payment, hủy/cập nhật đơn.
- `/api/reviews`: đánh giá và điểm trung bình sản phẩm.
- `/api/vouchers`: mã giảm giá.
- `/api/suppliers`, `/api/import-receipts`: nhà cung cấp và nhập hàng.
- `/api/warranties`: yêu cầu bảo hành.
- `/api/notifications`: thông báo.
- `/api/chat`, `/api/products/{productId}/discussions`: chat AI, hỗ trợ và hỏi đáp sản phẩm.
- `/api/reports`: dashboard và báo cáo.

Chi tiết endpoint và flow nghiệp vụ nằm trong `demo/API_FLOW.md`.

## Lệnh hữu ích

```bash
cd demo
./mvnw test
./mvnw -DskipTests compile
./mvnw clean package
```

Trên Windows PowerShell:

```powershell
cd demo
.\mvnw.cmd test
.\mvnw.cmd -DskipTests compile
.\mvnw.cmd clean package
```

## Kết nối với frontend

Frontend tương ứng: <https://github.com/truonggiang205/WatchStore_FE>

Khi chạy local, cấu hình frontend:

```env
VITE_API_BASE_URL=http://localhost:8080/api
```
