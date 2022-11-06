
[Về lại trang chủ](https://lehai2909.github.io)
# Giới thiệu về Micro-service - Docker - Kubernetes (Phần 4)

Chào các bạn. Đây sẽ là bài cơ bản cuối cùng trong loạt bài giới thiệu về Docker và Kubernetes của mình. Hi vọng bạn sẽ thích nó và tiếp tục ủng hộ mình trong những phần nâng cao hơn tiếp theo nhé :kissing:


Sau khi đã nắm cơ bản về những khái niệm như micro-service, Container hay Docker, mình tin là các bạn đã sẵn sàng để bước vào vùng đất cuối cùng mà chúng ta sẽ cùng nhau khám phá hôm nay: **Kubernetes**

Kubernetes là nền tảng mà nguồn mở dùng cho việc quản lý các dịch vụ hay ứng dụng được đóng gói (containerized). Được phát triển và mở mã nguồn bởi Google từ năm 2014, Kubernetes ngày nay đã trở thành một hệ sinh thái phát triển mạnh mẽ, trong đó bao gồm các dịch vụ và công cụ giúp đơn giản hóa việc phát triển, kiểm thử và triển khai các dịch vụ phần mềm, trên quy mô trải rộng từ doanh nghiệp nhỏ cho đến các công ty công nghệ toàn cầu

*Note*: Một vài điều thú vị có thể bạn chưa biết về Kubernetes:
- Kubernetes là nền tảng kế tiếp, được kế thừa từ **Borg**, một hệ thống nội bộ được giữ bí mật khá lâu của Google dùng để quản lý việc phát triển phần mềm và dịch vụ của Google
- K8s là tên gọi khác của Kubernetes. Lí do là vì trong tên gọi Kubernetes, giữa 2 chữ 'K' và 's' ở đầu và cuối, ta sẽ đếm được 8 chữ cái khác
- Kubernetes trong tiếng Hy Lạp có nghĩa là người lái tàu. Sẽ không ngạc nhiên nếu bạn biết logo của Kubernetes trông như thế này:

<img src="../images/Kubernetes/1200px-Kubernetes_logo_without_workmark.png" alt="kubernetes-logo" width="400" height='400' />

************************************************************************************************************

## Vai trò của Kubernetes

Đầu tiên, cần phải nhớ lại từ bài 1, mình đã đề cập về việc phát triển ứng dụng hiện nay chủ yếu đi theo hướng phát triển các dịch vụ con (micro-services). Đây là các dịch vụ hay ứng dụng thành phần được phát triển riêng biệt, nhưng vẫn có khả năng vận hành và trao đổi thông tin cùng nhau để tạo nên một ứng dụng hoàn chỉnh. 

Việc sử dụng công nghệ container để đóng gói các micro-service không chỉ mang đến sự tách biệt rõ ràng về chức năng, môi trường và tài nguyên, mà còn mang đến khả năng di động (portability) tuyệt vời cho các micro-service: chúng có thể được vận hành và quản lý tương tự nhau trong mọi môi trường: dù đó là trên máy chủ của công ty, hay trên các nền tảng đám mây như AWS hay GCP, hay bất kỳ chỗ nào mà container có thể đặt chân đến :man_astronaut:

Câu hỏi là: Kubernetes đóng vai trò gì ở đây?

Hãy quay lại với khái niệm về Kubernetes mình đã đề cập ở phần đầu: *"Kubernetes là nền tảng mà nguồn mở dùng cho việc quản lý các dịch vụ hay ứng dụng được đóng gói (containerized)*. Điều đó có nghĩa là ứng dụng của bạn, hay cụ thể hơn là các dịch vụ con thành phần của ứng dụng đã được đóng gói (các container) sẽ được quản lí và điều phối **tự động** bởi Kubernetes

Tại sao điều này lại quan trọng đến vậy? Để hiểu lí do tại sao, hãy tưởng tượng đến một thế giới không có K8s hay các dịch vụ tương tự. Bạn, với vai trò của một người phát triển ứng dụng, sẽ phải quản lí và theo dõi hoạt động của các container một cách thủ công: giả sử có một container nào đó ngừng hoạt động (do kết nối mạng gặp sự cố, hay do thiếu bộ nhớ RAM,...) bạn sẽ cần khởi chạy một container khác thay thế để việc cung cấp dịch vụ của container cũ không bị gián đoạn. Hay giả sử bạn là một nhà phát triển ứng dụng đang lên, giúp cho ứng dụng của bạn nhận được rất nhiều lượt truy cập. Và đến một thời điểm nào đó,  số lượng người dùng tăng cao vượt quá khả năng phục vụ của các container, và :collision:... Để tránh tình trạng ứng dụng của mình ngưng hoạt động do quá tải, bạn sẽ phải thực hiện việc mở rộng quy mô ứng dụng (scale) bằng cách khởi chạy các container mới giúp chia sẻ workload với các container hiện sắp quá tải.

Hãy tưởng tượng nếu bạn không phải chỉ quản lí 2 hay 3 container của một web app đơn giản. Chúng ta đang nói đến hàng ngàn container của một ứng dụng quy mô lớn, một điều khá bình thường với một ứng dụng có đến hàng triệu hay hàng tỉ người sử dụng, như Maps của Google chẳng hạn. Số lượng container lớn này đến từ:
- độ phức tạp của ứng dụng - ứng dụng càng phức tạp thì các dịch vụ thành phần càng nhiều
- mỗi một dịch vụ thành phần cần có nhiều container dự phòng, thuật ngữ gọi đây là redundancy (sự dư thừa). Mục đích của sự dư thừa này là để đảm bảo khả năng HA (High Availability) của hệ thống: mỗi khi có sự cố với một container, sẽ luôn có một container khác chạy thay thế trong thời gian khắc phục sự cố của container cũ

Bạn có muốn tự tay quản lí đống container đó không? 

![Image](../images/Kubernetes/withoutK.sketchpad.png)

Đó chính là lúc Kubernetes xuất hiện như một chiếc phao cứu sinh, giúp việc quản lý ứng dụng trở nên đơn giản và bớt "đau đớn" hơn rất nhiều. Kubernetes cung cấp một nền tảng phân tán giúp quản lý và đảm bảo sự vận hành liên tục của ứng dụng, thông qua việc khởi tạo, mở rộng hay chuyển đổi các container của ứng dụng một cách tự động tùy thuộc vào lưu lượng truy cập (traffic) vào ứng dụng. Ngoài chức năng quản lý container, Kubernetes còn cung cấp các chức năng về bảo mật (password, OAuth token,...) hay quản lý lưu trữ (trên cloud hay bộ lưu trữ cục bộ,...)

Để biết thêm về các tính năng khác của Kubernetes, bạn có thể truy cập [https://kubernetes.io](https://kubernetes.io/) để tìm hiểu thêm. Trang này cung cấp rất nhiều tutorial hữu ích giúp mình bắt đầu sử dụng Kubernetes từ cơ bản đến nâng cao :100:. Tuy nhiên, mình cũng cần nói thêm rằng, những tutorial này nhằm phục vụ mục đích tự học và tự vọc, nên ở đây sẽ chỉ có các công cụ cho phép bạn thiết lập và khởi tạo môi trường để chạy Kubernetes trên một máy tính local. Để thực sự sử dụng Kubernetes như một nền tảng phân tán trong môi trường production, mình khuyến khích bạn nên sử dụng dịch vụ Kubernetes được quản lí bởi các nền tảng điện toán đám mây như GCP hay AWS. Cá nhân mình đã sử dụng và khá ưng ý dịch vụ Kubernetes được cung cấp bởi GCP. Bạn có thể tham khảo ở link sau nhé: [https://cloud.google.com/kubernetes-engine](https://cloud.google.com/kubernetes-engine)

Cảm ơn bạn đã theo dõi mình đến đây. Thân ái và hẹn gặp lại ở những bài khác nhé ! :raised_hand_with_fingers_splayed:
