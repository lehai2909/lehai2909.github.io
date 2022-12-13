[Về lại trang chủ](https://lehai2909.github.io)
# Giới thiệu về Micro-service - Docker - Kubernetes (Phần 7)

# Workload trong Kubernetes

Workload, nói theo cách thường dân, là những ứng dụng được đóng gói được chạy trên K8S. Bạn đọc theo dõi những phần trước có lẽ vẫn nhớ về khái niệm **pod** mà mình đã [đề cập](https://lehai2909.github.io/blogs/microservice-kubernetes-docker-p5.html#c%C3%A1c-th%C3%A0nh-ph%E1%BA%A7n-c%E1%BB%A7a-m%E1%BB%99t-node-trong-k8s-cluster). Dù workload của bạn là gì đi nữa, chúng cũng sẽ được chạy bên trong các pod

Điều đó không có nghĩa là bạn cần phải đặt các workload của mình vào từng pod và quản lý chúng một cách riêng rẽ. K8s giúp cho cuộc sống của chúng ta dễ dàng hơn, bằng việc cung cấp các *workload resource*, thứ cho phép quản lý nhiều pod với số lượng và loại pod mong muốn. Workload resource sẽ quản lý các pod thay bạn thông qua việc khởi tạo và thiết đặt cho các đối tượng điều khiển (controller object)

Trước khi đào sâu vào những loại workload mà chúng ta có thể chạy trên K8S, hãy cùng nhau tìm hiểu về Pod trước!

## Pod

Pod, như đã đề cập lần thứ N, là đơn vị triển khai nhỏ nhất trên một cụm Kubernetes. Một Pod thường sẽ chứa một hoặc nhiều container chạy trong nó, nhưng một mô hình phổ biến là một pod thường chỉ chạy 1 container. Bên trong Pod, ta cũng có thể cài các volumes để lưu trữ giữ liệu cho các container ứng dụng.

## Pod Networking model

Làm sao các container trong Pod giao tiếp với nhau, cũng như giao tiếp với các container ở các pod khác? Đề làm được điều đó, Khi các pod được khởi tạo, chúng sẽ được tạo cho một network namespace riêng ở trên node, với cổng vào là một Virtual Network Interface (VNI) có địa chỉ IP được lấy từ dải địa chỉ phân bổ cho pod của cluster (xem hình dưới). Đây được gọi là mô hình "IP-per-pod" Ví dụ, pod bên trái trong hình dưới có địa chỉ IP: 10.255.255.101.

![microservice-diagram](../images/Kubernetes/networking-overview_single-node.png)

*(Tham khảo: https://cloud.google.com/kubernetes-engine/docs/concepts/network-overview)*

Điều thú vị là ở bên trong pod, tất cả các container đều kết nối đến VNI của pod (ví dụ ở trên là veth1234). Chúng nhìn pod như một máy tính vật lý bình thường với một network interface, và các container này có thể nói chuyện với nhau qua cổng localhost. VNI lại được kết nối đến VNI của pod khác(ở đây là veth5678) th, nhờ đó traffic có thể được lưu chuyển giữa các pod với nhau.

## Khởi tạo pod

Pod có thể được khởi tạo nhanh bằng cách sử dụng yaml manifest + công cụ kubectl. Một file manifest ví dụ như ở bên dưới:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```
Các bước thực hiện như sau:

+ Lưu file này với tên pod.yaml. Di chuyển terminal đến thư mục chưa file này, và chạy lệnh:
```kubectl apply -f pod.yaml```

Kết quả trả về thông báo pod đã được khởi tạo thành công:

---
<img width="635" alt="image" src="https://user-images.githubusercontent.com/49013652/206958112-595ce11c-5ddf-4676-8938-3405f4c014ad.png">

---

+ Bây giờ chạy lệnh: ``` kubectl get pod nginx -o wide```, chúng ta sẽ nhận được một số thông tin về pod, như địa chỉ ip và node mà trên đó pod này được khởi tạo:

---
<img width="1092" alt="image" src="https://user-images.githubusercontent.com/49013652/206959892-9e83d2d2-6c4d-4421-89b2-969d42c115cb.png">

---


