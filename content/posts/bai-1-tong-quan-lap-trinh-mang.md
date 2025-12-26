---
title: "Nhập môn Lập trình mạng với Java: Không khó như bạn nghĩ"
date: 2025-12-20
draft: false
tags: ["Java", "Network", "Socket", "Tutorial"]
cover:
  image: "https://images.unsplash.com/photo-1544197150-b99a580bb7a8"
  alt: "Network Programming"
  relative: false
comments: true
description: "Cùng tìm hiểu về IP, Port, Protocol và cách Java xử lý kết nối mạng thông qua ví dụ thực tế."
ShowToc: false
---

Hồi mới bắt đầu học lập trình, mình luôn tò mò: _"Làm sao hai cái máy tính cách xa nhau cả vòng trái đất lại có thể 'nói chuyện' với nhau được nhỉ?"_. Mình từng nghĩ nó là một thứ ma thuật gì đó rất phức tạp, chỉ dành cho các "pháp sư" quản trị mạng.

Nhưng khi đụng vào **Java**, cụ thể là gói `java.net`, mình mới vỡ lẽ: Hóa ra nó logic và gần gũi hơn mình tưởng. Hôm nay, mình sẽ chia sẻ lại những khái niệm cơ bản nhất về **Lập trình mạng (Network Programming)** theo cách dễ hiểu nhất, kèm theo code ví dụ để bạn có thể chạy thử ngay trên máy nhé!

## 1. Giải ngố các thuật ngữ (Concepts)

Trước khi viết code, chúng ta cần thống nhất với nhau về "ngôn ngữ" mà các máy tính sử dụng. Hãy tưởng tượng việc gửi dữ liệu qua mạng giống như việc bạn gửi một lá thư qua đường bưu điện vậy.

### IP (Internet Protocol) - Địa chỉ nhà

Nếu muốn gửi thư, bạn cần biết địa chỉ nhà người nhận. Trên mạng Internet, mỗi thiết bị (máy tính, điện thoại, server) đều có một định danh duy nhất gọi là **IP Address**.

- **Ví dụ:** `192.168.1.1` hay `172.217.14.206`.
- **Lưu ý:** Có IP Public (địa chỉ công khai trên Internet) và IP Private (địa chỉ trong mạng LAN nội bộ).

### Port (Cổng) - Số phòng

Biết địa chỉ nhà (IP) rồi, nhưng nhà đó là một chung cư có hàng trăm phòng thì sao? Làm sao bác đưa thư biết đưa cho ai?

- Ở đây, máy tính của bạn là cái chung cư, còn các ứng dụng (Web browser, Game, Skype...) là các căn phòng.
- **Port** chính là số phòng.
- **Thực tế:**
  - Web server thường "ngồi" ở phòng số **80** (HTTP) hoặc **443** (HTTPS).
  - Tomcat server mặc định thích phòng **8080**.
  - MySQL database lại thích phòng **3306**.
  - _Mẹo:_ Port có giá trị từ 0 đến 65535. Các port dưới 1024 thường dành cho hệ thống, code chơi chơi thì nên né ra nhé.

### Protocol (Giao thức) - Ngôn ngữ giao tiếp

Gặp được nhau rồi, nhưng hai người phải nói cùng một thứ tiếng mới hiểu nhau được. **Protocol** là bộ quy tắc quy định cách đóng gói và truyền dữ liệu.
Trong Java Network, bạn sẽ thường xuyên nghe đến 2 ông lớn:

1.  **TCP (Transmission Control Protocol):** Ông này cẩn thận, chắc chắn. Gửi cái gì là phải đảm bảo bên kia nhận được, đúng thứ tự, không mất mát. (Dùng cho web, email, chat).
2.  **UDP (User Datagram Protocol):** Ông này thì nhanh nhảu, phóng khoáng. Gửi là gửi, không quan tâm bên kia có nhận được hay không. (Dùng cho stream video, game online thời gian thực - nơi mà tốc độ quan trọng hơn sự chính xác tuyệt đối).

## 2. Thực hành: "Who am I?"

Lý thuyết thế đủ rồi, giờ hãy thử dùng Java để hỏi máy tính xem _"Tôi là ai và tôi đang ở đâu?"_ nhé. Chúng ta sẽ sử dụng class `InetAddress` trong gói `java.net`.

Đây là đoạn code mình hay dùng để test môi trường mạng:

```java
import java.net.InetAddress;
import java.net.UnknownHostException;

public class GetIP {
    public static void main(String[] args) {
        try {
            // 1. Lấy thông tin của chính máy đang chạy code (LocalHost)
            InetAddress myIP = InetAddress.getLocalHost();

            System.out.println("=== THÔNG TIN MÁY CỦA TÔI ===");
            // Tên định danh của máy trong mạng (Host Name)
            System.out.println("Hostname : " + myIP.getHostName());
            // Địa chỉ IP (Host Address)
            System.out.println("IP Address: " + myIP.getHostAddress());

            System.out.println("\n=== KIỂM TRA KẾT NỐI ===");
            // Thử kiểm tra xem có kết nối được tới Google không
            InetAddress google = InetAddress.getByName("[www.google.com](https://www.google.com)");
            System.out.println("Google IP: " + google.getHostAddress());
            System.out.println("Có kết nối được không? " + google.isReachable(3000)); // Timeout 3s

        } catch (UnknownHostException e) {
            System.err.println("Không tìm thấy địa chỉ máy! Kiểm tra lại mạng nhé.");
            e.printStackTrace();
        } catch (Exception e) {
            System.err.println("Có lỗi xảy ra: " + e.getMessage());
        }
    }
}
```

-- Giải thích code một chút:

- InetAddress.getLocalHost(): Phương thức static này trả về một đối tượng chứa thông tin IP và tên máy của bạn.
- getByName("url"): Đây là cách giúp bạn phân giải tên miền (DNS). Bạn đưa vào "https://www.google.com/search?q=google.com", Java sẽ trả về IP của Google.
- try-catch: Cực kỳ quan trọng. Khi làm việc với mạng, lỗi có thể xảy ra bất cứ lúc nào (rút dây mạng, sai IP, tường lửa chặn...). Luôn luôn phải bắt lỗi để chương trình không bị crash đột ngột.

3. Một số kinh nghiệm xương máu khi làm việc với java.net
   Sau một thời gian "vọc vạch", mình rút ra vài điểm các bạn cần lưu ý:

Đừng bao giờ Hardcode IP: IP có thể thay đổi (đặc biệt là trong mạng DHCP). Hãy dùng Hostname hoặc cấu hình IP trong file config riêng.

Cẩn thận với Port: Khi chạy ứng dụng server, nếu bạn gặp lỗi BindException: Address already in use, điều đó có nghĩa là cái Port bạn chọn đã bị ứng dụng khác chiếm (hoặc do lần chạy trước bạn chưa tắt hẳn chương trình). Đổi port khác hoặc kill process cũ là xong.

Firewall (Tường lửa) là kẻ thù: Nhiều khi code đúng 100%, chạy trên máy mình ngon lành, nhưng deploy lên server lại không kết nối được. 90% là do Firewall chặn Port. Hãy nhớ mở Port nhé!

4. Lời kết

Lập trình mạng trong Java là một cánh cửa mở ra vô vàn ứng dụng thú vị: từ làm app chat, game server, cho đến các hệ thống phân tán. Bài viết này mới chỉ là bước khởi đầu cưỡi ngựa xem hoa. Ở các bài sau, mình sẽ hướng dẫn các bạn làm một ứng dụng Chat Client-Server đơn giản bằng Socket.

Bạn đã chạy thử đoạn code trên chưa? IP máy bạn là gì? Comment bên dưới chia sẻ kết quả nhé!
