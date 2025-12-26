---
title: "Tạm biệt XMLHttpRequest, xin chào Fetch API: Cuộc cách mạng trong gửi request"
date: 2025-12-23
draft: false
tags: ["JavaScript", "Frontend", "API", "Tips"]
cover:
  image: "https://images.unsplash.com/photo-1555099962-4199c345e5dd"
  alt: "Code on screen"
  relative: false
ShowToc: false
description: "Tại sao chúng ta nên quên đi cách viết AJAX cũ kỹ và chuyển sang dùng Fetch API chuẩn hiện đại?"
---

Nếu bạn đã từng code web cách đây khoảng 5-7 năm, chắc chắn cái tên `XMLHttpRequest` sẽ gợi lại những ký ức... không mấy vui vẻ.

Ngày đó, để lấy một file JSON từ server về, chúng ta phải viết hàng đống dòng code, cấu hình đủ thứ sự kiện (`onreadystatechange`), rồi check mã lỗi thủ công. Nó cồng kềnh giống như việc bạn muốn nhắn tin cho bạn gái mà phải chạy ra bưu điện gửi thư tay vậy.

Nhưng may quá, thời thế thay đổi. Trình duyệt hiện đại đã trang bị cho chúng ta một vũ khí mới: **Fetch API**.

## 1. Nhìn lại quá khứ (để thấy mình sướng thế nào)

Đây là cách ngày xưa chúng ta lấy dữ liệu (AJAX cổ điển). Nhìn thôi đã thấy mệt rồi:

```javascript
// CÁCH CŨ (XMLHttpRequest) - Đừng học theo nhé!
const xhr = new XMLHttpRequest();
xhr.open(
  "GET",
  "[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)"
);
xhr.send();

xhr.onreadystatechange = function () {
  // Phải check đủ thứ điều kiện
  if (this.readyState === 4 && this.status === 200) {
    const data = JSON.parse(this.responseText); // Phải parse thủ công
    console.log(data);
  }
};
```

Bạn thấy đấy: Rối rắm, khó nhớ, và cực kỳ khó kết hợp (chaining) nhiều request lại với nhau.

## 2. Thời đại mới: Fetch API

Fetch ra đời dựa trên nền tảng của Promise (lời hứa). Cú pháp của nó tự nhiên hơn, giống như cách chúng ta nói chuyện vậy: "Ê trình duyệt, lấy (fetch) cho tao cái đường dẫn này, sau đó (then) chuyển nó thành JSON, sau đó (then) in ra màn hình."

Ví dụ thực tế: Lấy danh sách User
Hãy xem đoạn code trên được viết lại bằng Fetch gọn gàng thế nào:

```JavaScript

// CÁCH MỚI (Fetch API)
const url = '[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)';

fetch(url)
  .then(response => {
      // Fetch gửi về một "luồng" (stream), cần chuyển nó thành JSON
      return response.json();
  })
  .then(users => {
      // Ở đây chúng ta đã có mảng dữ liệu sạch sẽ
      console.log("✅ Đã lấy được " + users.length + " user:");

      users.forEach(user => {
          console.log(`- ${user.name} (${user.email})`);
      });
  })
  .catch(error => {
      // Bắt lỗi chung cho cả quá trình
      console.error("❌ Lỗi rồi: " + error);
  });
```

## 3. Cẩn thận: Một cái bẫy chết người của Fetch!

Có một điều cực kỳ quan trọng mà 90% người mới (và cả người cũ) hay nhầm lẫn:

Fetch sẽ KHÔNG nhảy vào catch() khi server trả về lỗi 404 (Not Found) hay 500 (Server Error).

Nó chỉ nhảy vào catch() khi mất mạng hoặc lỗi DNS mà thôi. Với Fetch, server trả về 404 vẫn được coi là "gửi request thành công".

Vì thế, code chuẩn chỉnh (Best Practice) phải là:

```JavaScript

fetch(url)
  .then(response => {
    // Phải tự kiểm tra thủ công property 'ok'
    if (!response.ok) {
        throw new Error("Lỗi Server trả về: " + response.status);
    }
    return response.json();
  })
  .then(data => { ... })
  .catch(err => {
      // Lúc này nó mới bắt được lỗi 404/500 mình ném ra ở trên
      console.error(err);
  });
```

## 4. Kết hợp với Async/Await (Combo hủy diệt)

Như bài trước mình đã chia sẻ về Async/Await, chúng ta có thể làm cho đoạn code trên đẹp hơn nữa, không còn .then() nối đuôi nhau:

```JavaScript

async function getUserList() {
try {
const response = await fetch(url);

        if (!response.ok) throw new Error("Lỗi tải trang!");

        const users = await response.json();
        console.log(users);

    } catch (error) {
        console.error("Xử lý lỗi tại đây:", error);
    }

}

```

## Lời kết

Fetch API giờ đây đã được hỗ trợ bởi hầu hết mọi trình duyệt (trừ Internet Explorer đã "xuống lỗ"). Nếu bạn đang học JavaScript hiện đại, hãy quên XMLHttpRequest đi và làm bạn thân với fetch ngay hôm nay nhé. Nó giúp code của bạn "thoáng" và dễ bảo trì hơn rất nhiều!
