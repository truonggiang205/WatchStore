# WatchStore Backend

Backend cho website ban dong ho WatchStore, xay dung bang Spring Boot. He thong cung cap API cho frontend quan ly san pham dong ho, gio hang, don hang, danh gia, voucher, bao hanh, nhap hang, nha cung cap, thong bao, chat ho tro va bao cao.

## Tinh nang chinh

- Xac thuc va phan quyen bang JWT cho cac vai tro `CUSTOMER`, `STAFF`, `OWNER`.
- Dang ky tai khoan khach hang voi OTP email, dang nhap bang username hoac email, quen mat khau va doi mat khau.
- Quan ly nguoi dung, khach hang, nhan vien va khoa/mo khoa tai khoan.
- Quan ly danh muc, san pham dong ho, anh san pham va so sanh san pham.
- Gio hang, dat hang, QR payment, huy don, cap nhat trang thai don hang va lich su trang thai.
- Danh gia san pham, voucher, nha cung cap va phieu nhap hang.
- Yeu cau bao hanh va quy trinh duyet/tu choi bao hanh.
- Thong bao he thong, chat AI, chat ho tro giua khach hang va nhan vien.
- Bao cao dashboard, doanh thu, don hang theo ngay/thang va san pham ban chay.

## Cong nghe

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

## Cau truc thu muc

```text
demo/
  src/main/java/com/example/demo/
    config/          # cau hinh security, CORS, JWT, AI, exception
    controllers/     # REST API controllers
    dtos/            # request/response DTO
    entities/        # JPA entities va enum
    exceptions/      # exception rieng
    repositories/    # Spring Data repositories
    services/        # business logic
  docs/              # tai lieu ky thuat va data SQL
  API_FLOW.md        # tong hop API va flow nghiep vu
```

## Yeu cau moi truong

- Java 17
- MySQL 8 hoac tuong duong
- Maven Wrapper da co san trong repo
- Tai khoan/dich vu tuy chon neu dung day du tinh nang: Cloudinary, SMTP mail, Google GenAI, Qdrant

## Cai dat va chay local

```bash
cd demo
./mvnw spring-boot:run
```

Tren Windows PowerShell:

```powershell
cd demo
.\mvnw.cmd spring-boot:run
```

API mac dinh chay tai:

```text
http://localhost:8080/api
```

## Cau hinh ung dung

Repo hien chua co file `src/main/resources/application.properties`, vi vay can tao file nay khi chay local. Vi du cau hinh toi thieu:

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

Cau hinh tuy chon cho cac tinh nang nang cao:

```properties
# Email OTP va email thong bao
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=your_email@gmail.com
spring.mail.password=your_app_password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
app.mail.from=${spring.mail.username}

# Cloudinary upload anh san pham
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

Ung dung dung Spring Data JPA voi MySQL. Co the de Hibernate tu tao/cap nhat schema bang:

```properties
spring.jpa.hibernate.ddl-auto=update
```

Neu can du lieu mau hoac tham khao cau truc du lieu, xem:

- `demo/docs/data_v2.sql`
- `data`

## API chinh

Base path: `/api`

- `/api/auth`: dang ky, xac thuc email, dang nhap, quen mat khau, reset mat khau.
- `/api/users`, `/api/customers`, `/api/staff`: quan ly tai khoan va nguoi dung.
- `/api/categories`, `/api/products`: danh muc, san pham, anh san pham va so sanh.
- `/api/cart`, `/api/orders`: gio hang, dat hang, QR payment, huy/cap nhat don.
- `/api/reviews`: danh gia va diem trung binh san pham.
- `/api/vouchers`: ma giam gia.
- `/api/suppliers`, `/api/import-receipts`: nha cung cap va nhap hang.
- `/api/warranties`: yeu cau bao hanh.
- `/api/notifications`: thong bao.
- `/api/chat`, `/api/products/{productId}/discussions`: chat AI, ho tro va hoi dap san pham.
- `/api/reports`: dashboard va bao cao.

Chi tiet endpoint va flow nghiep vu nam trong `demo/API_FLOW.md`.

## Lenh huu ich

```bash
cd demo
./mvnw test
./mvnw -DskipTests compile
./mvnw clean package
```

Tren Windows PowerShell:

```powershell
cd demo
.\mvnw.cmd test
.\mvnw.cmd -DskipTests compile
.\mvnw.cmd clean package
```

## Ket noi voi frontend

Frontend tuong ung: <https://github.com/truonggiang205/WatchStore_FE>

Khi chay local, cau hinh frontend:

```env
VITE_API_BASE_URL=http://localhost:8080/api
```
