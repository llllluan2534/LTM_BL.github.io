---
title: "Từ Frontend sang Backend: Hành trình viết API đầu tiên với Node.js & Express"
date: 2025-12-24
draft: false
tags: ["JavaScript", "Backend", "NodeJS", "Express", "Tutorial"]
cover:
  image: "https://images.unsplash.com/photo-1627398242454-45a1465c2479"
  relative: false
ShowToc: true
description: "Mình từng nghĩ làm Server khó lắm. Nhưng với Node.js, mình đã tự tay tạo ra API đầu tiên chỉ trong 5 phút. Và bạn cũng có thể!"
---

Hồi mới bắt đầu học lập trình web, mình cứ luẩn quẩn mãi ở phía Client (Frontend). Mình biết dùng HTML/CSS để vẽ giao diện, biết dùng `fetch()` để lấy dữ liệu về hiển thị.

Nhưng trong đầu mình luôn có một nỗi sợ và một câu hỏi lớn: **"Mấy cục dữ liệu JSON đó ở đâu chui ra vậy? Ai là người tạo ra nó?"**. Mình từng nghĩ viết Server là cái gì đó cao siêu lắm, phải học Java, C# hay những thứ phức tạp.

Cho đến khi mình tìm thấy **Node.js**.

Hóa ra, chúng ta có thể dùng chính JavaScript - ngôn ngữ mà anh em mình vẫn dùng để bắt sự kiện click nút - để viết Server. Cảm giác lần đầu tiên tự tay tạo ra một API, tự quyết định dữ liệu trả về nó "phê" không tả được. Hôm nay, mình sẽ chia sẻ lại trải nghiệm đó với các bạn.

## 1. Node.js và Express là cái gì? (Hiểu đơn giản thôi)

Trước khi code, mình thông não một chút nhé:

- **Node.js:** Bình thường JS chỉ chạy được trong trình duyệt (Chrome/Firefox). Node.js là môi trường giúp đem JS ra ngoài chạy trực tiếp trên máy tính (Server).
- **Express:** Nếu Node.js là cái động cơ xe, thì Express chính là cái khung xe, vô lăng, bánh xe... Nó là một bộ khung (Framework) giúp mình viết code server ngắn gọn hơn, đỡ phải cấu hình lằng nhằng.

## 2. Bước chuẩn bị: Dọn dẹp bàn làm việc

Để bắt đầu, bạn cần cài Node.js vào máy tính trước đã (lên trang chủ tải về cài như phần mềm bình thường nhé). Sau đó, hãy mở Terminal (hoặc CMD) và làm theo mình:

### Bước 2.1: Khởi tạo dự án

Tạo một thư mục mới, và gõ lệnh "thần thánh":

```bash
npm init -y
```

Lệnh này sẽ tạo ra một file package.json. Hãy coi nó như cái "Chứng minh thư" của dự án, chứa thông tin tên, phiên bản và các thư viện chúng ta sẽ dùng.

Bước 2.2: Cài đặt "trợ thủ" Express
Chúng ta sẽ không viết code chay từ con số 0 (hard mode), mà sẽ dùng Express cho nhàn.

```Bash

npm install express
```

Sau khi chạy xong, bạn sẽ thấy thư mục node_modules xuất hiện. Đừng bao giờ đụng vào nó, đó là nơi chứa hàng ngàn file thư viện mà Node tải về đấy.

3. Bắt tay vào Code: App.js
   Tạo ngay một file tên là app.js. Đây là nơi điều hành mọi hoạt động của Server.

Mình sẽ giải thích cặn kẽ từng dòng code dưới đây, vì hồi mới học mình cũng copy paste mà chả hiểu gì cả.

```JavaScript

// 1. Nhập khẩu (Import) thư viện Express
// Giống như việc bạn lấy bộ đồ nghề ra để chuẩn bị làm việc
const express = require('express');

// 2. Khởi tạo ứng dụng
// Biến 'app' này chính là đại diện cho cái Server của chúng ta
const app = express();

// 3. Chọn Cổng (Port)
// Server cũng giống như một tòa nhà. Để vào nhà, bạn cần biết đi vào cửa số mấy.
// Ở đây mình chọn cửa số 3000.
const port = 3000;

// =================================================
// 4. PHẦN QUAN TRỌNG NHẤT: ĐỊNH NGHĨA API (ROUTER)
// =================================================

// Cú pháp: app.method(đường_dẫn, hàm_xử_lý)
// Khi ai đó gõ vào trình duyệt địa chỉ: http://localhost:3000/api/hello
app.get('/api/hello', (req, res) => {

    console.log("🔔 Ting ting! Có ai đó vừa ghé thăm API của mình.");

    // req (Request): Chứa thông tin người gửi (họ gửi cái gì lên?)
    // res (Response): Công cụ để mình trả lời (mình gửi cái gì về?)

    // Trả về một cục JSON
    // Mẹo: Dùng res.json() sướng hơn res.send() vì nó tự động
    // gắn cái mác "Content-Type: application/json" cho mình.
    res.json({
        status: 200,
        message: "Xin chào! Đây là API đầu tay của tui viết bằng Node.js",
        data: {
            name: "Dev Đam Mê",
            skill: "Fullstack (Sắp thành)"
        }
    });
});

// =================================================

// 5. Bấm nút khởi động Server
// Lệnh này giúp Server luôn "lắng nghe" ở cái cổng 3000
app.listen(port, () => {
    // Lưu ý: Trong Node.js dùng console.log, đừng quen tay dùng System.out.println của Java nhé ^^!
    console.log(`🚀 Server đã phóng thành công!`);
    console.log(`👉 Truy cập thử ngay: http://localhost:${port}/api/hello`);
});
```

## 4. Giây phút sự thật: Chạy thử

Quay lại Terminal, gõ lệnh:

```Bash

node app.js
```

Nếu bạn thấy dòng chữ 🚀 Server đã phóng thành công! hiện lên, xin chúc mừng! Bạn đã chính thức có một Web Server đang chạy trên máy mình.

Giờ hãy mở Chrome lên và vào link: 'http://localhost:3000/api/hello'

Bùm! Bạn sẽ thấy một đoạn text JSON hiển thị trên màn hình. Đó không phải là file tĩnh, đó là dữ liệu được sinh ra từ code của bạn.

## 5. Một vấn đề nhỏ và giải pháp (Nodemon)

Hồi đầu mình gặp một cái rất ức chế: Mỗi khi mình sửa code (ví dụ đổi câu chào "Xin chào" thành "Hello"), mình f5 trình duyệt nhưng... nội dung không đổi!

Lý do là Node.js nạp code vào bộ nhớ một lần duy nhất lúc khởi động. Muốn cập nhật, bạn phải ra terminal bấm Ctrl + C để tắt server rồi chạy lại node app.js. Lặp đi lặp lại việc này chắc chắn sẽ làm bạn phát điên.

Bí kíp của mình: Hãy cài nodemon.

```Bash

npm install -g nodemon
```

Từ giờ, thay vì gõ node app.js, hãy gõ:

```Bash

nodemon app.js
Thằng nodemon này rất thông minh, nó sẽ canh chừng file code của bạn. Hễ bạn bấm Save (Lưu file) là nó tự khởi động lại Server ngay lập tức. Cực kỳ tiện lợi!
```

## Lời kết

- Vậy là hành trình của chúng ta đã đi một vòng khép kín:
- Hiểu về Mạng máy tính.
- Biết cách Client gọi dữ liệu (Fetch).
- Hiểu về dữ liệu (JSON).
- Và cuối cùng là tự tạo ra dữ liệu (Node.js API).

Tự viết được cái API đầu tiên cảm giác nó "quyền lực" lắm các bạn ạ. Từ nay frontend cần gì, mình chiều được hết. Hy vọng bài chia sẻ này sẽ giúp các bạn bớt sợ Backend và tự tin hơn trên con đường trở thành Fullstack Developer.
