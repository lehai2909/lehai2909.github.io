[Về lại trang chủ](https://lehai2909.github.io)
# Giới thiệu về Micro-service - Docker - Kubernetes (Phần 7)

# Workload trong Kubernetes

Workload, nói theo cách thường dân, là những ứng dụng được đóng gói được chạy trên K8S. Bạn đọc theo dõi những phần trước có lẽ vẫn nhớ về khái niệm **pod** mà mình đã [đề cập](https://lehai2909.github.io/blogs/microservice-kubernetes-docker-p5.html#c%C3%A1c-th%C3%A0nh-ph%E1%BA%A7n-c%E1%BB%A7a-m%E1%BB%99t-node-trong-k8s-cluster). Dù workload của bạn là gì đi nữa, chúng cũng sẽ được chạy bên trong các pod

Điều đó không có nghĩa là bạn cần phải đặt các workload của mình vào từng pod và quản lý chúng một cách riêng rẽ. K8s giúp cho cuộc sống của chúng ta dễ dàng hơn, bằng việc cung cấp các *workload resource*, thứ cho phép quản lý nhiều pod với số lượng và loại pod mong muốn. Workload resource sẽ quản lý các pod thay bạn thông qua việc khởi tạo và thiết đặt cho các đối tượng điều khiển (controller object)

Trước khi đào sâu vào những loại workload mà chúng ta có thể chạy trên K8S, hãy cùng nhau tìm hiểu về Pod trước!

## Pod

Pod, như đã đề cập lần thứ N, là đơn vị triển khai nhỏ nhất trên một cụm Kubernetes. Một Pod thường sẽ chứa một hoặc nhiều container chạy trong nó (tham khảo [link](https://kubernetes.io/docs/concepts/workloads/pods/#workload-resources-for-managing-pods)), nhưng một mô hình phổ biến là một pod thường chỉ chạy 1 container. Bên trong Pod, ta cũng có thể cài các volumes để lưu trữ và chia sẻ dữ liệu cho các container.

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


<img width="635" alt="image" src="https://user-images.githubusercontent.com/49013652/206958112-595ce11c-5ddf-4676-8938-3405f4c014ad.png">

---

+ Bây giờ chạy lệnh: ``` kubectl get pod nginx -o wide```, chúng ta sẽ nhận được một số thông tin về pod, như địa chỉ ip và node mà trên đó pod này được khởi tạo:


<img width="1092" alt="image" src="https://user-images.githubusercontent.com/49013652/206959892-9e83d2d2-6c4d-4421-89b2-969d42c115cb.png">

---

Giờ thì chúng ta đã tạo đuợc thành công một pod, nhưng làm sao để nhìn vào bên trong của pod và container của nó? kubectl cung cấp một câu lệnh hữu ích giúp chúng ta tương tác với container, như tạo ra một shell chạy trong container để chạy các lệnh, giống như khi ta đang có một terminal của linux vậy. Đó là lệnh ```kubectl exec```.

Quay lại với terminal đang làm việc lúc nãy, bạn hãy chạy dòng lệnh sau:
```kubectl exec -it nginx -- bash```

Một cách thần kỳ, một shell sẽ được chạy, cho phép bạn thao tác các câu lệnh bên trong container, sau đây là một ví dụ khi bạn chạy lệnh ```pwd``` trong shell vừa được mở:

<img width="658" alt="image" src="https://user-images.githubusercontent.com/49013652/210166022-d4905a96-972c-4c4b-a160-02539502f20c.png">

---
Để tham khảo về ý nghĩa các thành phần của câu lệnh, bạn có thể xem thêm tại (đây)[https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec]


## Chia sẻ tài nguyên lưu trữ trong pod
Một pod có thể chỉ định một volume chung để lưu trữ dữ liệu. Trong trường hợp khi ta chạy nhiều container trong một pod, các container đều có quyền truy cập đến volume này, bằng cách mount volume này vào filesystem của container. Sau đây là một ví dụ về việc một pod sử dụng một volume loại EmptyDir để phục vụ việc chia sẻ dữ liệu giữa 2 container bên trong pod, được khai báo dưới dạng file .yaml:

```
apiVersion: v1
kind: Pod
metadata:
  name: two-containers
spec:
  volumes:
  - name: shared-data
    emptyDir: {}
  containers:
  - name: nginx-container
    image: nginx
    volumeMounts:
    - name: **shared-data**
      mountPath: /usr/share/nginx/html
  - name: debian-container
    image: debian
    volumeMounts:
    - name: shared-data
      mountPath: /pod-data
    command: ["/bin/sh"]
    args: ["-c", "echo Hello from the debian container > /pod-data/index.html"]
```
Bạn chưa cần biết về các loại volume trong K8S (mình sẽ trình bày về nó trong phần về Storage trong K8s), bạn chỉ cần hiểu sơ là ở đây, chúng ta đang có 2 container: ```nginx-container``` và ```debian-container```. 2 container này mount một volume có tên ```shared-data``` tại các thư mục với đường dẫn tương ứng là ```/usr/share/nginx/html``` và ```/pod-data``` trong từng filesystem của container. Vì đây là một shared volume, nên khi bạn tạo hoặc thay đổi nội dung file trong một thư mục, sự thay đổi đó cũng sẽ xuất hiện ở thư mục còn lại.

## Deployment
Việc sử dụng một pod để chạy ứng dụng, đem đến một nguy cơ là khi pod này gặp vấn đề và ngừng hoạt động (điều rất dễ xảy ra khi traffic của ứng dụng tăng cao dẫn đến cạn kiệt tài nguyên cpu/ram,..), ứng dụng cũng sẽ ngừng chạy, và người dùng sẽ bị ảnh hưởng. Một điều bất lợi nữa là pod không có cơ chế re-schedule sau khi ngừng hoạt động, vậy nên có thể làm gián đoạn ứng dụng trong một thời gian nếu pod thay thế không được tạo lại (bằng tay)
