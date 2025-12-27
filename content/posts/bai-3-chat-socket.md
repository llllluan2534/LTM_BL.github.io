---
title: "Tutorial: Code ứng dụng Chat 'Hello World' trong 5 phút"
date: 2025-12-21
draft: false
tags: ["Java", "Socket", "Project", "Beginner", "Tutorial"]
categories: ["Java", "Network"]
cover:
  image: "https://static.wixstatic.com/media/17c24e_cfb842fec21d49258622a115abc20d04~mv2.png/v1/fit/w_1000%2Ch_544%2Cal_c/file.png"
  alt: "Client Server Code"
  relative: false
comments: true
description: "Đừng chỉ in ra màn hình console nữa. Hãy gửi lời chào qua mạng! Hướng dẫn chi tiết từng bước xây dựng kết nối TCP đầu tiên."
ShowToc: false
---

Nếu bạn đã chán ngấy việc viết `System.out.println("Hello World")` và chỉ một mình bạn nhìn thấy, thì hôm nay chúng ta sẽ nâng cấp nó lên một tầm cao mới: **"Hello World" qua mạng**.

Trong bài này, mình sẽ cùng bạn xây dựng mô hình Client-Server kinh điển nhất. Đây là bài vỡ lòng bắt buộc cho bất kỳ ai muốn làm Backend, Game Server hay IoT.

## 1. Cơ chế hoạt động: Đường ống nước

Trước khi code, hãy tưởng tượng kết nối mạng giống như một **hệ thống ống nước** nối giữa hai nhà (Máy A và Máy B).

- **Socket:** Là cái vòi nước ở hai đầu.
- **InputStream (Luồng nhập):** Là dòng nước chảy **VÀO** nhà bạn (để bạn hứng nước/nhận dữ liệu).
- **OutputStream (Luồng xuất):** Là dòng nước chảy **RA** khỏi nhà bạn (để bạn bơm nước/gửi dữ liệu đi).

**Quy tắc vàng:** `OutputStream` của máy này sẽ nối thẳng vào `InputStream` của máy kia. Server "nói" (Write) thì Client "nghe" (Read) và ngược lại.

## 2. Server Side (Người phục vụ)

Server có nhiệm vụ mở một cái "Cổng" (Port) và ngồi thiền ở đó chờ đợi.

```java
import java.io.*;
import java.net.*;

public class SimpleServer {
    public static void main(String[] args) {
        // Chọn cổng 5000 (Bạn có thể chọn số khác, nhưng tránh 0-1023 ra nhé)
        int port = 5000;

        try (ServerSocket server = new ServerSocket(port)) {
            System.out.println("✅ Server đã khởi động!");
            System.out.println("⏳ Đang chờ khách tại port " + port + "...");

            // QUAN TRỌNG: Lệnh .accept() sẽ "đóng băng" chương trình tại đây
            // Nó sẽ treo máy cho đến khi có một Client kết nối vào.
            Socket socket = server.accept();

            // Khi dòng này chạy, tức là đã có người kết nối!
            System.out.println("🎉 Có khách kết nối từ: " + socket.getInetAddress());

            // --- BẮT ĐẦU GIAO TIẾP ---

            // Lấy luồng xuất để gửi tin nhắn đi
            OutputStream os = socket.getOutputStream();
            DataOutputStream dos = new DataOutputStream(os);

            System.out.println("📨 Đang gửi lời chào...");
            dos.writeUTF("Chào Client, tôi là Server đây! Rất vui được gặp bạn.");

            System.out.println("✅ Đã gửi xong. Đóng kết nối.");

            // Đóng socket để giải phóng tài nguyên (Cực kỳ quan trọng)
            socket.close();

        } catch (IOException e) {
            System.err.println("Lỗi Server: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## 3. Client Side (Khách hàng)

Client đóng vai trò chủ động. Nó cần biết Server đang ở đâu (IP) và cổng nào (Port) để "gõ cửa".

```Java

import java.io.*;
import java.net.*;

public class SimpleClient {
    public static void main(String[] args) {
        // "localhost" nghĩa là máy của chính mình (IP 127.0.0.1)
        // Nếu Server ở máy khác, hãy điền IP của máy đó (VD: "192.168.1.5")
        String serverIP = "localhost";
        int port = 5000;

        try {
            System.out.println("📞 Đang gọi tới Server " + serverIP + ":" + port + "...");

            // Dòng này sẽ cố gắng kết nối. Nếu Server chưa mở, nó sẽ báo lỗi ngay.
            Socket socket = new Socket(serverIP, port);

            System.out.println("✅ Đã kết nối thành công!");

            // --- BẮT ĐẦU GIAO TIẾP ---

            // Lấy luồng nhập để đọc tin nhắn Server gửi tới
            InputStream is = socket.getInputStream();
            DataInputStream dis = new DataInputStream(is);

            // Đọc tin nhắn (Chương trình cũng sẽ đợi ở đây nếu Server chưa gửi gì)
            String message = dis.readUTF();

            System.out.println("💬 Server nhắn: " + message);

            socket.close();

        } catch (ConnectException e) {
            System.err.println("❌ Không thể kết nối! Hãy chắc chắn bạn đã chạy Server trước.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 4. Hướng dẫn chạy (Run) đúng cách

Rất nhiều bạn mới học bị bối rối ở bước này vì chúng ta có tới... 2 hàm main.

**Cách thực hiện**:

`Bước 1`: Chạy file SimpleServer.java trước.

Bạn sẽ thấy dòng ⏳ Đang chờ khách tại port 5000... hiện lên console.

Lúc này chương trình vẫn đang chạy, đừng tắt nó đi nhé!

`Bước 2`: Mở một cửa sổ console mới (hoặc tab terminal mới trong IDE), chạy file SimpleClient.java.

`Bước 3`: Quan sát kết quả.

Bên Client sẽ hiện: 💬 Server nhắn: Chào Client...

Bên Server sẽ hiện: 🎉 Có khách kết nối... sau đó tự đóng.

## 5. Các lỗi thường gặp (Troubleshooting)

Lập trình mạng là nơi sinh ra vô số lỗi kỳ quặc. Dưới đây là 2 lỗi phổ biến nhất:

Lỗi 1: java.net.ConnectException: Connection refused.

- Hiện tượng: Chạy Client và bị báo lỗi đỏ lòm ngay lập tức.

**Nguyên nhân**: Bạn chưa chạy Server, hoặc Server đang chạy ở port khác, hoặc Firewall chặn.

**Khắc phục**: Chạy lại Server. Kiểm tra xem số Port ở 2 file code có giống nhau không (đều là 5000).

Lỗi 2: java.net.BindException: Address already in use

- Hiện tượng: Chạy Server và bị báo lỗi này.

**Nguyên nhân**: Cổng 5000 đang bận. Có thể do lần chạy trước bạn chưa tắt Server hẳn, nó vẫn đang chạy ngầm. Hoặc một phần mềm khác (như Docker, Skype) đang chiếm cổng này.

**Khắc phục**: Tắt các tiến trình Java đang chạy (nút Stop đỏ trong IDE). Hoặc đổi port sang số khác (VD: 9999).

## Thử thách nhỏ (Mini Challenge)

Hiện tại Server chỉ nói 1 câu rồi... "ngắt máy". Bạn hãy thử sửa code để:

Client gửi lại một câu: "Chào Server, mình nhận được rồi nhé!".

Server nhận câu đó và in ra màn hình.

Gợi ý: Dùng dos.writeUTF() ở bên Client và dis.readUTF() ở bên Server.

Làm được thì comment kết quả bên dưới cho mình biết nhé! Chúc các bạn code vui!
