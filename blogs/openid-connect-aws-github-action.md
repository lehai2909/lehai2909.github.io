[Về lại trang chủ](https://lehai2909.github.io)

# OPEN ID CONNECT LÀ GÌ? (DEMO CI/CD VỚI GITHUB ACTION)?

Khi xây dựng một CI/CD pipeline để triển khai (deploy) một ứng dụng, thông thường, pipeline cần phải có quyền truy cập đến các tài nguyên trên cloud (ví dụ như AWS, Azure,...). 

Ví dụ, pipeline sẽ cần quyền truy cập đến EKS để cập nhật deployment, hoặc cần quyền truy cập đến S3 nếu bạn build và lưu trữ static website trên S3. Một hình ảnh đơn giản mô tả quá trình deploy một ứng dụng container chạy trên EKS sẽ như thế này:


![openid-connect drawio (1)](https://github.com/user-attachments/assets/f1c05b0f-a62d-4d2b-a292-d9c3ab3ea340)


Để pipeline có được quyền truy cập này, thông thường cách đơn giản nhất là người dùng cần phải cài đặt các credential (access keys) trong môi trường CICD. Việc này thường được thực hiện bằng cách lưu access key vào các secret trong CICD pipeline. Điều này khá tương tự giữa các platform như Gitlab CICD, Github Action hay Jenkins.


Tuy nhiên, điều này cũng đồng nghĩa với việc người dùng cần phải thực hiện bằng tay việc khởi tạo các access key trên môi trường cloud, cũng như lưu trữ chúng lâu dài trong môi trường CICD.

AWS khuyến khích người dùng không thực hiện việc lưu trữ long-term credential như vậy. Thay vào đó họ khuyến khích người dùng thiết lập ứng dụng của mình sử dụng **OIDC federation** để yêu cầu short-time credential khi cần, và các credential này sẽ tự động hết hạn sau một khoảng thời gian định sẵn. Tham khảo [ở đây](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc.html)
