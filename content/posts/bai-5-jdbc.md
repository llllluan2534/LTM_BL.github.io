---
title: "JDBC & MySQL: Biến ứng dụng Chat từ 'Cá vàng' thành 'Voi nhớ lâu'"
date: 2025-12-22
draft: false
tags: ["Java", "Database", "JDBC", "SQL", "Security"]
categories: ["Java", "Network"]
cover:
  image: "https://images.unsplash.com/photo-1544383835-bda2bc66a55d"
  relative: false
comments: true
description: "Server tắt là mất hết tin nhắn? Không đâu. Hướng dẫn kết nối Database chuẩn chỉnh và an toàn với JDBC."
ShowToc: false
---

Ở các bài trước, Server của chúng ta đã biết chat, biết phân luồng xử lý nhiều khách. Nhưng nó mắc một căn bệnh nan y: **Trí nhớ cá vàng**. Chỉ cần bạn tắt Server hoặc (xui hơn) là cúp điện, toàn bộ tin nhắn, tài khoản người dùng sẽ "bay màu" vĩnh viễn vì chúng đang được lưu trên RAM.

Để ứng dụng có một "bộ nhớ vĩnh cửu", chúng ta cần lưu dữ liệu xuống ổ cứng thông qua một **Hệ quản trị cơ sở dữ liệu (DBMS)** như MySQL, PostgreSQL hay SQL Server.

Trong thế giới Java, cây cầu nối giữa code và database tên là **JDBC (Java Database Connectivity)**.

## 1. Giải ngố: JDBC hoạt động thế nào?

Java và MySQL giống như hai người nói hai ngôn ngữ khác nhau.

- Java nói tiếng... Java (Object, Class).
- MySQL nói tiếng SQL (Table, Row, Query).

Để 2 ông này hiểu nhau, chúng ta cần một **"Người phiên dịch"**, đó chính là **JDBC Driver**.

Quy trình giao tiếp gồm 3 bước:

1.  **Load Driver:** Thuê người phiên dịch.
2.  **Create Connection:** Thiết lập đường dây nóng (gọi điện).
3.  **Execute Statement:** Gửi câu lệnh (nhờ làm việc) và nhận kết quả.

## 2. Bước 0: Chuẩn bị "Đồ nghề" (Driver)

Rất nhiều bạn mới học bị lỗi ngay bước đầu tiên vì... quên tải Driver. Java không có sẵn Driver của MySQL, bạn phải tự thêm vào.

### Cách 1: Dành cho dân chuyên (Khuyên dùng)

Nếu bạn dùng Maven hoặc Gradle (mà năm 2025 rồi, hãy dùng đi nhé), bạn chỉ cần copy đoạn này vào file cấu hình:

**Maven (`pom.xml`):**

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
</dependency>
**Gradle (build.gradle):
```

**Groovy**

```
implementation 'mysql:mysql-connector-java:8.0.33'
```

### Cách 2: Thủ công (Cho bài tập nhỏ)

Tải file .jar từ trang chủ MySQL, sau đó vào IDE (IntelliJ/Eclipse) -> Project Structure -> Libraries -> Add file .jar vừa tải vào.

## 3. Code kết nối chuẩn (Best Practice)

Chúng ta sẽ không hardcode (viết cứng) thông tin vào code một cách bừa bãi. Hãy làm mọi thứ chỉnh chu ngay từ đầu.

```Java
import java.sql.*;

public class DBConnection {
    // 1. Thông tin cấu hình
    // Cấu trúc URL: jdbc:loại_db://địa_chỉ_ip:cổng/tên_database
    private static final String URL = "jdbc:mysql://localhost:3306/ChatAppDB";
    private static final String USER = "root";
    private static final String PASS = "mat_khau_cua_ban"; // Đừng để lộ cái này nhé!

    public static void main(String[] args) {

        System.out.println("⏳ Đang kết nối tới Database...");

        // 2. Sử dụng try-with-resources
        // Cú pháp try(...) này giúp tự động đóng kết nối (conn.close()) dù có lỗi xảy ra.
        // Giúp tránh rò rỉ bộ nhớ (Memory Leak) cực tốt.
        try (Connection conn = DriverManager.getConnection(URL, USER, PASS)) {

            System.out.println("✅ Kết nối thành công!");

            // --- THỰC HIỆN TRUY VẤN ---
            checkLogin(conn, "admin", "secret123");
            checkLogin(conn, "hacker", "' OR '1'='1"); // Thử hack xem sao?

        } catch (SQLException e) {
            System.err.println("❌ Lỗi kết nối: " + e.getMessage());
            System.err.println("Kiểm tra lại: 1. MySQL đã bật chưa? 2. Tên DB đúng không? 3. Sai mật khẩu?");
        }
    }

    // Hàm kiểm tra đăng nhập tách riêng
    private static void checkLogin(Connection conn, String username, String password) throws SQLException {
        // 3. Tại sao dùng PreparedStatement thay vì Statement thường?
        // SQL: SELECT * FROM users WHERE username = ? AND password = ?
        String sql = "SELECT * FROM users WHERE username = ? AND password = ?";

        PreparedStatement pstmt = conn.prepareStatement(sql);

        // Điền dữ liệu vào các dấu hỏi chấm (?)
        pstmt.setString(1, username);
        pstmt.setString(2, password);

        // Thực thi và nhận kết quả
        ResultSet rs = pstmt.executeQuery();

        if (rs.next()) {
            System.out.println("🔓 Đăng nhập thành công cho user: " + username);
        } else {
            System.out.println("🔒 Sai tài khoản hoặc mật khẩu!");
        }
    }
}
```

## 4. Tại sao phải dùng PreparedStatement?

Bạn có thể thấy mình dùng PreparedStatement và các dấu ?. Tại sao không cộng chuỗi cho nhanh như thế này?

```Java

// ĐỪNG BAO GIỜ LÀM THẾ NÀY:
String sql = "SELECT * FROM users WHERE user = '" + username + "'";
```

Nếu bạn làm vậy, hacker sẽ nhập username là: ' OR '1'='1. Khi đó câu lệnh SQL trở thành: SELECT \* FROM users WHERE user = '' OR '1'='1'. Vì 1=1 luôn đúng, hacker sẽ đăng nhập thành công mà không cần mật khẩu!. Đây là lỗi SQL Injection kinh điển.

PreparedStatement sẽ coi mọi thứ người dùng nhập vào là văn bản thuần túy, vô hiệu hóa các ký tự đặc biệt, giúp ứng dụng của bạn an toàn tuyệt đối.

## 5. Các lỗi thường gặp (Troubleshooting)

Làm việc với DB thì lỗi là chuyện cơm bữa. Đừng hoảng, hãy check checklist sau:

ClassNotFoundException: Chưa thêm thư viện (Driver) vào project (Xem lại mục 2).

Communications link failure: MySQL Server chưa bật, hoặc sai IP/Port.

Access denied for user: Sai username hoặc password.

Unknown database: Sai tên database (kiểm tra lại trong MySQL Workbench/phpMyAdmin xem tạo DB chưa).

## Lời kết

Vậy là bạn đã nắm trong tay chìa khóa để lưu trữ dữ liệu. Server của chúng ta giờ đây đã có "trí nhớ".

Trong bài viết tiếp theo (và cũng là bài cuối của series cơ bản), chúng ta sẽ gộp tất cả lại: Một Server Đa luồng + Có Database + Client giao diện dòng lệnh để tạo ra một Chat Room hoàn chỉnh. Hẹn gặp lại!
