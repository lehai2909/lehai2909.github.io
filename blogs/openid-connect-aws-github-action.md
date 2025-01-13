[Về lại trang chủ](https://lehai2909.github.io)

# OPEN ID CONNECT LÀ GÌ? (DEMO CI/CD VỚI GITHUB ACTION)?

Khi xây dựng một CI/CD pipeline để triển khai (deploy) một ứng dụng, thông thường, pipeline cần phải có quyền truy cập đến các tài nguyên trên cloud (ví dụ như AWS, Azure,...). 

Ví dụ, pipeline sẽ cần quyền truy cập đến EKS để cập nhật deployment, hoặc cần quyền truy cập đến S3 nếu bạn build và lưu trữ static website trên S3. Một hình ảnh đơn giản mô tả quá trình deploy một ứng dụng container chạy trên EKS sẽ như thế này:


![openid-connect drawio (1)](https://github.com/user-attachments/assets/f1c05b0f-a62d-4d2b-a292-d9c3ab3ea340)


Để pipeline có được quyền truy cập này, thông thường cách đơn giản nhất là người dùng cần phải cài đặt các credential (access keys) trong môi trường CICD. Việc này thường được thực hiện bằng cách lưu access key vào các secret trong CICD pipeline. Điều này khá tương tự giữa các platform như Gitlab CICD, Github Action hay Jenkins.
