[Về lại trang chủ](https://lehai2909.github.io)

# OPEN ID CONNECT LÀ GÌ? (DEMO CI/CD VỚI GITHUB ACTION)?

Khi xây dựng một CI/CD pipeline để triển khai (deploy) một ứng dụng, thông thường, pipeline cần phải có quyền truy cập đến các tài nguyên trên cloud (ví dụ như AWS, Azure,...). 

Ví dụ, pipeline sẽ cần quyền truy cập đến EKS để cập nhật deployment, hoặc cần quyền truy cập đến S3 nếu bạn build và lưu trữ static website trên S3. Một hình ảnh đơn giản mô tả quá trình deploy một ứng dụng container chạy trên EKS sẽ như thế này:


![openid-connect drawio (1)](https://github.com/user-attachments/assets/f1c05b0f-a62d-4d2b-a292-d9c3ab3ea340)


Để pipeline có được quyền truy cập này, thông thường cách đơn giản nhất là người dùng cần phải cài đặt các credential (access keys) trong môi trường CICD. Việc này thường được thực hiện bằng cách lưu access key vào các secret trong CICD pipeline. Điều này khá tương tự giữa các platform như Gitlab CICD, Github Action hay Jenkins.


Tuy nhiên, điều này cũng đồng nghĩa với việc người dùng cần phải thực hiện bằng tay việc khởi tạo các access key trên môi trường cloud, cũng như lưu trữ chúng lâu dài trong môi trường CICD.

AWS khuyến khích người dùng không thực hiện việc lưu trữ long-term credential như vậy. Thay vào đó họ khuyến khích người dùng thiết lập ứng dụng của mình sử dụng **OIDC federation** để yêu cầu short-time credential khi cần, và các credential này sẽ tự động hết hạn sau một khoảng thời gian định sẵn. Tham khảo [ở đây](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc.html)


Để hiểu việc này được thực hiện như thế nào, trước tiên chúng ta cần hiểu về 2 khái niệm: **OAuth 2.0** và **OpenID Connect**.


## OAuth 2.0

OAuth 2.0 là một cơ chế xác thực (authorization framework) cho phép một ứng dụng bên ngoài có quyền truy cập vào các tài nguyên dưới danh nghĩa của bạn, mà bạn không cần phải cung cấp cho ứng dụng này những thông tin xác thực (credential) nhạy cảm, như username và password.

Hãy thử lấy một ví dụ: Bạn đang sử dụng một ứng dụng web (ở đây gọi là client). Để sử dụng các tính năng chỉ dành cho thành viên, trang web sẽ yêu cầu bạn phải đăng nhập. Bạn có thể đăng ký (Sign-up) rồi điền các thông tin username(email) và password để đăng nhập, hoặc, trang web này sẽ cung cấp tính năng **Login with Google** hoặc **Login with Github** để bạn có thể đăng nhập vào trang web sử dụng accout Google hoặc Github của bạn:

![image](https://github.com/user-attachments/assets/491d2dad-6b4e-4fc2-80a9-c6ebfaf4cbef)

Trong trường hợp này, *chúng ta đang cho phép ứng dụng bên ngoài truy cập vào các thông tin tài khoản (như email Google, ảnh đại diện,...) mà không hề cung cấp cho ứng dụng các thông tin xác thực (email/password) dùng để đăng nhập vào tài khoản cá nhân*

Vậy điều này diễn ra như thế nào? Làm thế nào một ứng dụng không biết email/password của tôi lại có thể truy cập (một cách hạn chế) vào các thông tin tài khoản của tôi :anguished:?



![image](https://github.com/user-attachments/assets/da408497-fa39-4177-a645-9bc7bcd47e09)


![image](https://github.com/user-attachments/assets/7b79913b-a6c6-465c-85a9-89217591a6e4)


![image](https://github.com/user-attachments/assets/9620797b-7475-42f0-9cf6-f863f165a7ab)


