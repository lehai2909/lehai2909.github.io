[Về lại trang chủ](https://lehai2909.github.io)

# Hành trình tự học lập trình web 1: CORS là gì?

Cùng tìm hiểu về **Cross-origin resource sharing (CORS)**, và cách cấu hình CORS thông qua một ứng dụng web đơn giản

![image](https://github.com/lehai2909/lehai2909.github.io/assets/49013652/b3be5676-20d6-4f91-b004-7dc20da3b360)

## CORS là gì?

Trong mô hình client-server, CORS là cơ chế dựa trên HTTP header, cho phép các server có thể cho phép hoặc ngăn chặn client từ một domain khác có thể truy cập đến tài nguyên trên server của mình hay không. Nói cách khác, CORS là cách để một client (ví dụ như một web app, được host trên một domain) có thể tương tác với tài nguyên trên một domain khác.

Một hình ảnh rất dễ hiểu đuợc lấy từ [MDN mozilla web](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS):

![image](https://github.com/lehai2909/lehai2909.github.io/assets/49013652/f69952a0-baeb-4594-8ca9-e79b7c5d99e7)

 
Tại sao điều này lại quan trọng? Đơn giản là vì khi xây dựng một trang web, thông thường trang web của bạn cần phải truy cập tới các third-party API. Một vài ví dụ như sau:
- Khi bạn muốn thay đổi font chữ của trang web trong CSS/HTML, bạn cần sử dụng font chữ từ một thư viện mở (ví dụ như [google font](https://fonts.google.com/))
- Khị bạn muốn sử dụng một link hình ảnh public trên Internet...
