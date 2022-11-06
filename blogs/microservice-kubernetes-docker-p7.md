[Về lại trang chủ](https://lehai2909.github.io)
# Giới thiệu về Micro-service - Docker - Kubernetes (Phần 7)

# Workload trong Kubernetes

Workload, nói theo cách thường dân, là những ứng dụng được đóng gói được chạy trên K8S. Bạn đọc theo dõi những phần trước có lẽ vẫn nhớ về khái niệm **pod** mà mình đã [đề cập](https://lehai2909.github.io/blogs/microservice-kubernetes-docker-p5.html#c%C3%A1c-th%C3%A0nh-ph%E1%BA%A7n-c%E1%BB%A7a-m%E1%BB%99t-node-trong-k8s-cluster). Dù workload của bạn là gì đi nữa, chúng cũng sẽ được chạy bên trong các pod

Điều đó không có nghĩa là bạn cần phải đặt các workload của mình vào từng pod và quản lý chúng một cách riêng rẽ. K8s giúp cho cuộc sống của chúng ta dễ dàng hơn, bằng việc cung cấp các *workload resource*, thứ cho phép quản lý nhiều pod với số lượng và loại pod mong muốn. Workload resource sẽ quản lý các pod thay bạn thông qua việc khởi tạo và thiết đặt cho các đối tượng điều khiển (controller object)

Trước khi đào sâu vào những loại workload mà chúng ta có thể chạy trên K8S, hãy cùng nhau tìm hiểu về Pod trước!

## Pod
