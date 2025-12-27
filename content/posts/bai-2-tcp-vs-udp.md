---
title: "TCP và UDP: Cuộc chiến không hồi kết của thế giới mạng"
date: 2025-12-21
draft: false
tags: ["Java", "Network", "Theory"]

categories: ["Java", "Network"]
cover:
  image: "https://gloat.com/wp-content/uploads/2023/10/shutterstock_1998159614.jpg"
  relative: false
comments: true
description: "Nên chọn sự tin cậy hay tốc độ? Một bài so sánh vui vẻ để bạn không bao giờ nhầm lẫn giữa hai giao thức này nữa."
ShowToc: false
---

Khi bước chân vào lập trình mạng, câu hỏi "triệu đô" mà ai cũng phải đối mặt là: **Nên dùng TCP hay UDP?**

Để dễ hình dung, hãy tưởng tượng:

- **TCP** giống như một cuộc **gọi điện thoại**. Bạn (Client) gọi, người kia (Server) bắt máy ("Alo"), rồi hai bên mới bắt đầu nói chuyện. Nếu tín hiệu kém, bạn sẽ hỏi lại: _"Alo, nghe rõ không?"_ để đảm bảo thông tin không bị mất.
- **UDP** giống như việc **bắn tin nhắn SMS** (hoặc gửi thư). Bạn cứ soạn và gửi đi. Người nhận có nhận được hay không, hoặc tin nhắn đến trước hay đến sau, bạn không quan tâm và cũng chẳng kiểm soát được.

## 1. Bảng so sánh (Phiên bản chi tiết)

[Image of TCP vs UDP communication flow diagram]

| Đặc điểm          | TCP (Transmission Control Protocol)                                     | UDP (User Datagram Protocol)                                          |
| :---------------- | :---------------------------------------------------------------------- | :-------------------------------------------------------------------- |
| **Triết lý**      | _"Chậm mà chắc"_                                                        | _"Nhanh và nguy hiểm"_                                                |
| **Cơ chế**        | Bắt tay 3 bước (3-way handshake) thiết lập kết nối trước khi truyền.    | Bắn dữ liệu (Datagram) đi ngay lập tức, không cần kết nối.            |
| **Độ tin cậy**    | **Tuyệt đối.** Gói tin bị lỗi/mất sẽ được gửi lại. Đảm bảo đúng thứ tự. | **Thấp.** Mất gói tin thì thôi, không gửi lại. Thứ tự có thể lộn xộn. |
| **Tốc độ**        | Chậm hơn do thủ tục rườm rà.                                            | Rất nhanh, header gói tin nhỏ gọn.                                    |
| **Dùng khi nào?** | Web (HTTP), Email (SMTP), Chat, Tải file (FTP).                         | Game FPS, Livestream, Video Call (mất vài khung hình cũng không sao). |

## 2. Trong Java thì code thế nào?

Java cung cấp bộ công cụ riêng biệt cho hai ông này:

- **Team TCP:** Bạn sẽ làm việc với `Socket` (đại diện cho một kết nối) và `ServerSocket` (để lắng nghe kết nối). Mọi thứ hoạt động dựa trên luồng (Stream) - `InputStream` và `OutputStream`.
- **Team UDP:** Bạn sẽ dùng `DatagramSocket` để gửi/nhận và `DatagramPacket` để đóng gói dữ liệu.

**Lời khuyên:** Nếu mới bắt đầu làm ứng dụng Chat hoặc gửi File, hãy chọn **TCP** cho "lành". Khi nào làm game bắn súng thời gian thực hãy nghĩ tới UDP nhé!
