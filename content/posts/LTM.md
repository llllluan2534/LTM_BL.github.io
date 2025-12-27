---
title: "Lớp Lập trình mạng: Khi những dòng code Socket khô khan được 'thổi hồn' bởi thầy Nguyễn Quang Trung"
date: 2025-12-26
draft: false
tags:
  [
    "University Life",
    "Network Programming",
    "Review",
    "Inspiration",
    "Java Socket",
  ]
cover:
  image: "https://file1.hutech.edu.vn/file/editor/homepage1/18712-ntd_2078.jpg"
  relative: false
comments: true
description: "Từ nỗi sợ những tầng OSI trừu tượng đến niềm vui khi Client-Server kết nối thành công. Một hành trình đầy cảm xúc với thầy Trung, kẹo ngọt và AWS."
---

Nếu có ai hỏi mình môn học nào ở đại học là "ác mộng" về độ khô khan và trừu tượng, chắc chắn nhiều người sẽ nghĩ ngay đến **Lập trình mạng (Network Programming)**.

Thú thật, trước khi đăng ký, mình đã chuẩn bị sẵn tâm lý để "đánh vật" với những mớ lý thuyết mạng chằng chịt, những tầng OSI chồng chéo, hay những đoạn code Socket Java dài ngoằng chỉ để gửi một dòng "Hello World". Nhưng may mắn thay, người dẫn đường cho mình trong học kỳ này là **thầy Nguyễn Quốc Trung**. Và thầy đã chứng minh cho mình thấy: Lập trình mạng không hề khô khan, nó thực sự sống động nếu chúng ta biết cách tiếp cận đúng.

Hôm nay, mình muốn viết một bài review thật tâm huyết, không chỉ về người thầy tuyệt vời này mà còn về chính môn học đã thay đổi tư duy lập trình của mình.

## 1. Nỗi tiếc nuối mang tên "Viên kẹo buổi đầu tiên"

Kỷ niệm đầu tiên của mình về lớp học này lại là một... sự vắng mặt (nghĩ lại vẫn thấy tiếc hùi hụi 😅). Hôm khai giảng, mình có việc bận nên phải vắng học. Cứ đinh ninh buổi đầu chắc thầy chỉ lên đọc slide, phổ biến quy chế rồi về như bao môn khác.

Nhưng tối đó về nhà, lướt group lớp thấy tụi bạn rần rần: _"Trời ơi thầy Trung dễ thương xỉu, nay thầy mang cả đống kẹo vào mời từng bạn ăn để làm quen á!"_.
Mình sững người. Ở cái môi trường đại học mà giảng viên và sinh viên thường giữ một khoảng cách vô hình, việc một người thầy bước vào lớp với những viên kẹo ngọt ngào là điều quá đỗi hiếm hoi. Hành động ấy như một lời khẳng định: _"Môn này khó, nhưng lớp chúng ta sẽ học một cách vui vẻ, ngọt ngào chứ không đắng ngắt đâu!"_. Dù chưa gặp mặt, mình đã cảm nhận được cái tâm của thầy đặt vào lớp học này lớn đến thế nào.

## 2. Khi "Lập trình mạng" không còn là những khái niệm trên mây

Quay lại với chuyên môn. Ai học Lập trình mạng rồi sẽ biết, cái khó nhất của nó là **tính trừu tượng**. Dữ liệu chạy từ máy A sang máy B như thế nào? Tại sao tin nhắn gửi đi lại mất? Giao thức TCP bắt tay 3 bước ra sao?

Bình thường đọc sách giáo khoa, mình chỉ thấy toàn chữ là chữ. Nhưng trong giờ của thầy Trung, những khái niệm đó được "bình dân hóa" một cách triệt để nhờ **Phương pháp Feynman** và **tư duy thực tế**.

Thầy không bắt tụi mình học vẹt định nghĩa _"TCP là giao thức hướng kết nối..."_. Thay vào đó, thầy dạy tụi mình cách tưởng tượng.

- Thầy ví việc thiết lập kết nối **TCP** giống như một cuộc gọi điện thoại: _"Alo, nghe rõ không?" - "Ừ, nghe rõ" - "Ok, nói chuyện nhé"_.
- Còn **UDP** thì giống như gửi một lá thư tay hay bắn một tin nhắn SMS: Gửi là gửi, tới hay không thì... hên xui.
- Thầy ví **Socket** như cái bến cảng, còn **Port** như số hiệu cầu tàu để tàu bè (gói tin) biết đường mà cập bến.

Nhờ cách dạy trực quan đó, những đoạn code `ServerSocket`, `InputStream`, `OutputStream` không còn là những dòng lệnh vô tri nữa. Mình bắt đầu hình dung được luồng dữ liệu chảy trong dây mạng như dòng nước trong ống. Mỗi khi code bị lỗi `ConnectionRefused` hay `BindException`, thay vì hoảng loạn copy lỗi lên Google, mình đã biết cách bình tĩnh tư duy: _"À, chắc là chưa mở cổng, hay là cổng này đang có thằng khác chiếm dụng..."_. Đó là sự trưởng thành về tư duy mà mình trân trọng nhất.

## 3. Vượt qua nỗi sợ và những cánh tay rụt rè

Mình vốn là đứa hướng nội, rất sợ cảm giác đứng lên phát biểu mà nói sai, sợ quê với bạn bè. Nhưng thầy Trung đã xây dựng một văn hóa lớp học tuyệt vời: **Sai để hiểu sâu hơn**.

Mỗi lần thầy đặt câu hỏi về một vấn đề hóc búa (ví dụ: _"Trong quá trình này, có bao nhiêu luồng hoạt động"_), thầy luôn nhìn quanh lớp chờ đợi những cánh tay. Sau khi cũng có một bạn giơ tay và trả lời cũng gần gần đúng.
Đúng lúc đó thì tự nhiên một vươn tay thế là thầy đã thấy và gọi mình trả lời, cũng may là trước đó, mình đã kịp tìm hiểu câu hỏi trên. Và kết quả là mình đã trả lời hoàn toàn đúng, vậy nên thầy đã tặng cho mình một điểm cộng và một tràn vỗ tay từ cả lớp. Wow lúc đó mình kiểu: \_"amazing gút chóp"

Sự động viên khích lệ đó khiến mình cảm thấy an toàn. Mình nhận ra lớp học Lập trình mạng này là nơi để tư duy, để tranh luận chứ không phải nơi để bị dò bài. Từ đó, mình tự tin hơn hẳn, dám nói, dám sai và dám sửa.

## 4. Mang cả "đại dương" AWS vào lớp học nhỏ

Không chỉ dừng lại ở lý thuyết sách vở hay những dòng code Java Socket thuần túy, thầy Trung còn mở rộng tầm mắt cho tụi mình bằng những kinh nghiệm thực chiến từ cộng đồng AWS (Amazon Web Services).

Thầy cho tụi mình thấy rằng "Lập trình mạng" ngày nay không chỉ là chuyện nối dây LAN trong phòng thực hành nữa. Nó là Cloud Computing, là hàng triệu server kết nối với nhau trên toàn cầu. Thầy hay kể về những bài toán thực tế mà doanh nghiệp đang đau đầu: Làm sao để hệ thống chịu tải triệu view? Load Balancing hoạt động thế nào? Môi trường làm việc của các kỹ sư mạng/DevOps bên ngoài khốc liệt nhưng thú vị ra sao?

Đỉnh điểm của sự "thực chiến" này là sự kết hợp giữa thầy, Khoa và CLB Mạng Máy Tính để tổ chức chuỗi 6 chuyên đề chuyên sâu về AWS.

Dù hôm diễn ra chuyên đề đầu tiên: "Cloud Basics, AWS Overview & Cost Fundamentals", mình lỡ hẹn không tham gia được (tiếc hùi hụi luôn!), nhưng nghe thầy review lại mà thấy rạo rực thay. Thấy bảo hôm đó các anh chị cựu sinh viên – những người đang làm việc trực tiếp trong ngành – đã chia sẻ cực kỳ tâm huyết.

Nội dung không chỉ là cưỡi ngựa xem hoa, mà đi sâu vào những thứ cốt lõi nhất:

Hệ thống hóa kiến thức chuẩn mực: Giúp sinh viên hiểu rõ bản chất Cloud là gì, và nắm vững các dịch vụ lõi như Compute, Storage, Networking, Database... Đây chính là những mảnh ghép quan trọng để tụi mình học tốt các môn chuyên ngành sau này.

Bài toán "Đau ví" (Cost Fundamentals): Cái này là thực tế nhất nè! Sinh viên tụi mình muốn vọc vạch Cloud nhưng sợ nhất là... trừ tiền thẻ tín dụng. Buổi workshop đã hướng dẫn chi tiết cách quản trị chi phí, cách dùng gói AWS Free Tier sao cho an toàn và hiệu quả. Nghe tụi bạn kể xong mà mình thấy yên tâm hẳn, không còn nỗi lo "mất tiền oan" khi làm lab nữa.

Những câu chuyện đó như liều "doping" tinh thần, giúp mình hiểu rằng những dòng code `socket.accept()` mình viết hôm nay chính là nền móng cho những hệ thống vĩ mô sau này.

## Lời kết

Kết thúc môn học, khi nhìn lại project Chat Client-Server cuối kỳ chạy ngon lành, mình cảm thấy thực sự biết ơn.

- Biết ơn vì thầy đã biến một môn học khô khan thành niềm cảm hứng.
- Biết ơn phương pháp Feynman đã giúp mình hiểu sâu bản chất vấn đề.
- Và biết ơn những lời động viên đã giúp một đứa rụt rè như mình dám cất tiếng nói.

Cảm ơn thầy Nguyễn Quốc Trung vì đã là người truyền lửa đúng nghĩa. Nếu các bạn khóa sau thấy tên thầy trong danh sách đăng ký, đừng chần chừ. Hãy chuẩn bị một tâm hồn đẹp (và đi học buổi đầu đầy đủ để được ăn kẹo nha) để đón nhận những kiến thức tuyệt vời nhé!
