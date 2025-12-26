---
title: "Tại sao Server cần Đa luồng? Giải quyết bài toán 'Quán cà phê một nhân viên'"
date: 2025-12-22
draft: false
tags: ["Java", "Thread", "Concurrency", "Server Architecture"]
cover:
  image: "https://images.unsplash.com/photo-1550751827-4bd374c3f58b"
  relative: false
comments: true
description: "Server của bạn sẽ 'chết đứng' nếu chỉ dùng một luồng. Cùng tìm hiểu mô hình Multi-threaded để phục vụ hàng ngàn người cùng lúc."
ShowToc: false
---

Trong bài trước, chúng ta đã code thành công một Server biết nói "Hello". Tuy nhiên, nếu bạn thử chạy Server đó và cho 2 người bạn cùng kết nối vào, một "thảm họa" sẽ xảy ra: **Người thứ 2 sẽ bị treo máy cho đến khi người thứ 1 thoát hẳn.**

Tại sao lại như vậy? Hãy cùng mổ xẻ vấn đề dưới góc nhìn của một quán cà phê.

## 1. Vấn đề: Quán cà phê "Độc mã" (Single-threaded)

Hãy tưởng tượng bạn mở một quán Starbug (lỗi đánh máy, ý mình là Starbucks), nhưng cả quán chỉ có **đúng một nhân viên duy nhất** (đây chính là `Main Thread`).

Quy trình phục vụ sẽ diễn ra như sau:

1.  **Khách A** bước vào.
2.  Nhân viên chạy ra chào, đưa menu.
3.  Nhân viên đứng đợi Khách A chọn món (trong lập trình, đây là đoạn `dis.readUTF()` - server dừng lại để chờ dữ liệu).
4.  Nhân viên đi pha chế, bưng ra, đợi khách uống xong, tính tiền, tiễn khách về.
5.  **MỚI ĐẾN LƯỢT KHÁCH B.**

Trong suốt khoảng thời gian Khách A ngồi nhâm nhi cafe (hoặc suy nghĩ chọn món), Khách B, C, D... phải đứng xếp hàng ngoài cửa dưới trời nắng. Đây chính là cơ chế **Blocking I/O**. Trong thực tế, không ai chấp nhận một dịch vụ như vậy cả.

## 2. Giải pháp: Mô hình "Lễ tân & Phục vụ" (Multi-threaded)

Để giải quyết vấn đề này, chúng ta không thể bắt một nhân viên làm tất cả mọi việc. Chúng ta cần chia nhiệm vụ:

- **1 Lễ tân (Main Thread):** Chỉ đứng ở cửa, việc duy nhất là cười tươi và đón khách (`accept()`). Ngay khi có khách vào, Lễ tân sẽ gọi một nhân viên phục vụ ra.
- **N Nhân viên phục vụ (Worker Threads):** Mỗi nhân viên sẽ chăm sóc riêng cho một bàn (Client). Khách bàn nào bàn nấy lo, không ảnh hưởng đến nhau.

Khi đó, Lễ tân ngay lập tức rảnh tay để đón tiếp người tiếp theo, bất kể người khách trước đó ngồi lâu đến mức nào.

## 3. Triển khai Code (Full Code)

Chúng ta cần tách logic "chăm sóc khách hàng" ra một class riêng, gọi là `ClientHandler`. Class này sẽ thực thi interface `Runnable` để có thể chạy trên một luồng riêng biệt.

### Phần 1: ClientHandler (Nhân viên phục vụ)

```java
import java.io.*;
import java.net.*;

// Class này chịu trách nhiệm phục vụ MỘT khách hàng duy nhất
public class ClientHandler implements Runnable {
    private Socket socket;

    public ClientHandler(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        try {
            // Tạo luồng nhập/xuất
            DataInputStream dis = new DataInputStream(socket.getInputStream());
            DataOutputStream dos = new DataOutputStream(socket.getOutputStream());

            // Gửi lời chào
            dos.writeUTF("Chào mừng! Bạn đang được phục vụ bởi nhân viên số: " + Thread.currentThread().getId());

            // Vòng lặp để chat liên tục (thay vì nói 1 câu rồi tắt)
            while (true) {
                // Đợi khách nhắn tin (Block ở đây, nhưng chỉ block luồng này thôi)
                String messageFromClient = dis.readUTF();

                System.out.println("Khách " + socket.getPort() + " nói: " + messageFromClient);

                if (messageFromClient.equals("bye")) {
                    dos.writeUTF("Tạm biệt nhé!");
                    break; // Thoát vòng lặp để đóng kết nối
                }

                // Phản hồi lại
                dos.writeUTF("Server đã nhận: " + messageFromClient);
            }

            // Đóng kết nối khi khách rời đi
            socket.close();
            System.out.println("Khách " + socket.getPort() + " đã ngắt kết nối.");

        } catch (IOException e) {
            System.err.println("Khách ngắt kết nối đột ngột!");
        }
    }
}
```

## Phần 2: MultiThreadServer (Lễ tân)

```Java

import java.io.*;
import java.net.*;

public class MultiThreadServer {
    public static void main(String[] args) {
        int port = 5000;

        try (ServerSocket server = new ServerSocket(port)) {
            System.out.println("🏢 Server đã mở cửa tại port " + port);
            System.out.println("👨‍💼 Lễ tân đang chờ khách...");

            // Vòng lặp vô tận để server luôn sống
            while (true) {
                // 1. Lễ tân chờ khách (Block tại đây cho đến khi có kết nối mới)
                Socket socket = server.accept();
                System.out.println("👋 Khách mới ghé thăm: " + socket);

                // 2. Tuyển nhân viên mới (Tạo Thread)
                ClientHandler worker = new ClientHandler(socket);
                Thread t = new Thread(worker);

                // 3. Đẩy nhân viên ra làm việc
                t.start();

                // 4. Lễ tân quay lại đầu vòng lặp while để chờ người tiếp theo ngay lập tức!
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 4. Phân tích sâu: Điều gì thực sự diễn ra?

Khi bạn chạy đoạn code trên và thử kết nối bằng 2 Client cùng lúc:

- Main Thread chạy đến server.accept() và dừng lại chờ.

- Client 1 kết nối. Main Thread tỉnh dậy, tạo ra Thread A để xử lý Client 1.

- t.start() được gọi:

- Thread A: Bắt đầu chạy hàm run(), nhảy vào vòng lặp chờ tin nhắn từ Client 1.

- Main Thread: Không quan tâm Thread A làm gì, nó quay ngay lại server.accept() để chờ khách tiếp theo.

- Client 2 kết nối. Main Thread lại tỉnh dậy, tạo Thread B.

-> Lúc này, Thread A và Thread B chạy song song (concurrently). Client 1 chat không ảnh hưởng gì đến Client 2.

## 5. Góc nhìn Senior: Cái giá phải trả

Mặc dù Đa luồng giải quyết được vấn đề "xếp hàng", nhưng nó không miễn phí.

Tốn RAM: Mỗi một Thread được tạo ra trong Java (mặc định) sẽ ngốn khoảng 1MB bộ nhớ cho Stack. Nếu có 10.000 người kết nối cùng lúc, bạn mất 10GB RAM chỉ để quản lý các luồng rỗng.

Context Switching: CPU phải nhảy qua nhảy lại giữa các luồng để xử lý. Quá nhiều luồng sẽ khiến CPU "chóng mặt" và hiệu năng giảm sút.

Giải pháp cho tương lai: Thay vì new Thread() vô tội vạ mỗi khi có khách, các hệ thống lớn sử dụng Thread Pool (Hồ chứa luồng). Tức là tuyển sẵn 100 nhân viên, ai rảnh thì làm, bận thì khách phải chờ một chút. Hoặc cao cấp hơn là dùng mô hình Non-blocking I/O (NIO).

Nhưng đó là chuyện của tương lai. Hiện tại, chúc mừng bạn đã xây dựng được một Server "xịn sò" có thể tiếp khách đoàn rồi đấy!
