# Giới thiệu về Micro-service - Docker - Kubernetes (Phần 4)

Chào các bạn. Đây sẽ là bài cuối cùng trong loạt bài giới thiệu về Docker và Kubernetes của mình. Hi vọng bạn sẽ thích nó và tiếp tục ủng hộ mình nhé :kissing:

Sau khi đã nắm cơ bản về những khái niệm như micro-service, Container hay Docker, mình tin là các bạn đã sẵn sàng để bước vào vùng đất cuối cùng mà chúng ta sẽ cùng nhau khám phá hôm nay: **Kubernetes**

Kubernetes là nền tảng mà nguồn mở dùng cho việc quản lý các dịch vụ hay ứng dụng được đóng gói (containerized). Được phát triển và mở mã nguồn bởi Google từ năm 2014, Kubernetes ngày nay đã trở thành một hệ sinh thái phát triển mạnh mẽ, trong đó bao gồm các dịch vụ và công cụ giúp đơn giản hóa việc phát triển, kiểm thử và triển khai các dịch vụ phần mềm, trên quy mô trải rộng từ doanh nghiệp nhỏ cho đến các công ty công nghệ toàn cầu

*Note*: Một vài điều thú vị có thể bạn chưa biết về Kubernetes:
- Kubernetes là nền tảng kế tiếp, được kế thừa từ **Borg**, một hệ thống nội bộ được giữ bí mật khá lâu của Google dùng để quản lý việc phát triển phần mềm và dịch vụ của Google
- K8s là tên gọi khác của Kubernetes. Lí do là vì trong tên gọi Kubernetes, giữa 2 chữ 'K' và 's' ở đầu và cuối, ta sẽ đếm được 8 chữ cái khác
- Kubernetes trong tiếng Hy Lạp có nghĩa là người lái tàu. Sẽ không ngạc nhiên nếu bạn biết logo của Kubernetes trông như thế này:

<img src="../images/1200px-Kubernetes_logo_without_workmark.png" alt="kubernetes-logo" width="400" height='400' />

(đang bận tay ăn cơm, xin thông cảm :zany_face:)
