---
title: "Tâm sự chuyện Async/Await: Cách mình thoát khỏi 'địa ngục' Callback"
date: 2025-12-23
draft: false
tags: ["JavaScript", "Story", "Experience", "Tips"]
categories: ["JavaScript"]
cover:
  image: "https://images.unsplash.com/photo-1579468118864-1b9ea3c0db4a"
  relative: false
comments: true
description: "Hồi mới học JS, mình từng phát điên vì code chạy lung tung không theo thứ tự. Và đây là cách mình kiểm soát nó."
ShowToc: false
---

Thú thật với mọi người, hồi mới chuyển từ C/Java sang học JavaScript, mình bị "sốc văn hóa" nặng.

Mình cứ đinh ninh là code viết dòng 1 xong thì phải đến dòng 2. Nhưng không, trong JS, nhiều khi dòng 2 chạy xong rồi dòng 1 mới chịu trả về kết quả (nhất là mấy cái gọi API hay `setTimeout`). Lúc đó mình kiểu: _"Ủa, cái ngôn ngữ gì kỳ cục vậy?"_ 🤯.

Sau này mới hiểu, đó là tính năng (feature), không phải lỗi (bug). Đó là cơ chế **Bất đồng bộ (Asynchronous)**. Hôm nay mình sẽ chia sẻ lại cách mình tư duy về nó để anh em đỡ bỡ ngỡ nhé.

## 1. JavaScript: Anh chàng phục vụ bàn "siêu tốc"

Mọi người hay bảo JS là **Single-thread** (đơn luồng), tức là chỉ có 1 tay 1 chân, làm từng việc một. Vậy sao nó xử lý được hàng nghìn request cùng lúc?

Hãy tưởng tượng JS giống như một **anh bồi bàn** trong quán cafe cực đông khách:

- **Cách làm dở (Đồng bộ):** Anh ta nhận order của khách A -> Đứng chờ pha chế làm xong -> Bưng ra cho A -> Rồi mới quay lại nhận order của khách B.
  - => _Kết quả:_ Khách B chờ dài cổ, quán sập tiệm.
- **Cách JS làm (Bất đồng bộ):** Anh ta nhận order của A -> Quăng tờ giấy vào bếp (Web API) hô to "Làm đi nhé!" -> Quay ngoắt sang nhận order của B ngay lập tức.
  - => _Kết quả:_ Anh ta không bao giờ đứng chơi, bếp làm xong món nào thì anh ta bưng món đó ra.

Cái cơ chế "nhờ bếp làm hộ" rồi quay lại xử lý sau đó chính là chìa khóa giúp JS mượt mà dù chỉ có 1 luồng.

## 2. Ngày xưa chúng tôi khổ thế nào? (Callback Hell)

Hồi chưa có `async/await`, để xử lý việc "khi nào bếp làm xong thì bưng ra", tụi mình dùng **Callback** (hàm gọi lại).

Ví dụ: Muốn lấy User -> rồi lấy Bài viết -> rồi lấy Comment. Code nó sẽ trông như cái kim tự tháp lật ngang thế này:

```javascript
// Đây là ác mộng của mọi JS Dev
getData(function (a) {
  getMoreData(a, function (b) {
    getMoreData(b, function (c) {
      getMoreData(c, function (d) {
        getMoreData(d, function (e) {
          console.log("Xong rồi nè!");
        });
      });
    });
  });
});
```

Anh em thấy kinh khủng không? Code này cực khó đọc, khó sửa lỗi (debug). Người ta gọi đây là Callback Hell (Địa ngục Callback).

## 3. "Chân ái" xuất hiện: Async/Await

Rồi ES6 ra đời với Promise, đỡ hơn chút. Nhưng phải đến ES8 với async/await, cuộc đời mình mới thực sự nở hoa.

Nó cho phép mình viết code bất đồng bộ mà nhìn y chang như code tuần tự bình thường. Cảm giác viết code nó "phê" như viết văn xuôi vậy.

Hãy xem sự khác biệt nhé:

```JavaScript

// Phải có từ khóa 'async' ở đầu hàm nhé
async function layDuLieu() {
  try {
    console.log("1. Đang gọi API...");

    // Từ khóa 'await' bảo JS: "Ê, dừng ở đây đợi chút, khi nào có kết quả thì chạy tiếp"
    let response = await fetch("[https://api.example.com/data](https://api.example.com/data)");

    // Đợi convert sang JSON
    let data = await response.json();

    console.log("2. Đã có dữ liệu:", data);

  } catch (error) {
    // Xử lý lỗi y hệt như Java/C#
    console.log("Toang rồi: " + error);
  }
}
```

Nhìn sạch sẽ hơn hẳn đúng không? Không còn callback, không còn .then(), .catch() rối rắm nữa.

## 4. Một vài kinh nghiệm xương máu

Dùng async/await sướng thật, nhưng có một cái bẫy mà hồi mới dùng mình hay mắc phải: Lạm dụng await khiến code chạy chậm như rùa.

Ví dụ: Mình cần lấy thông tin của 3 user.

❌ Cách sai (Chạy lần lượt):

```JavaScript

// Mất tổng cộng 3 giây (nếu mỗi cái mất 1s)
let user1 = await getUser(1);
let user2 = await getUser(2); // Phải chờ ông 1 xong mới chạy
let user3 = await getUser(3); // Phải chờ ông 2 xong mới chạy
✅ Cách đúng (Chạy song song):

JavaScript

// Chỉ mất 1 giây thôi!
// Bắn 3 request đi cùng lúc
let p1 = getUser(1);
let p2 = getUser(2);
let p3 = getUser(3);

// Đợi cả 3 ông cùng về đích
let [user1, user2, user3] = await Promise.all([p1, p2, p3]);
```

## Lời kết

Nếu bạn đang học JS và thấy rối rắm với mấy cái .then() hay callback, lời khuyên chân thành của mình là hãy học chắc Async/Await ngay đi. Nó là tiêu chuẩn bây giờ rồi.

Code không chỉ để máy chạy, mà còn để người đọc. Viết sao cho đồng nghiệp (và chính mình 1 tháng sau) đọc lại không muốn "đấm vào màn hình" là thành công rồi! 😄
