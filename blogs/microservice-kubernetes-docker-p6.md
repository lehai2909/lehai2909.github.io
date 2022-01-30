# Giới thiệu về Micro-service - Docker - Kubernetes (Phần 6)


*Lưu ý về các phần mềm cơ bản cần thiết để follow phần này*

Để tìm hiểu về Kiến trúc của K8S, bạn sẽ cần cài đặt một số phần mềm cơ bản sau:

- [minikube](https://minikube.sigs.k8s.io/docs/start/), một phần mềm giúp khởi tạo một K8S cluster chạy nội bộ trên máy tính (gồm các hệ điều hành MacOS, Linux hay Windows). minikube cho phép khởi tạo cluster với chỉ **một** node (Master-node) và không có các worker-node. Vì vậy, Minikube chỉ phù hợp cho việc học và thử nghiệm cơ bản về Kubernetes

- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/), một chương trình giúp quản lý Kubernetes cluster. Cũng giống minikube, kubectl cũng có thể được sử dụng trên nhiều hệ điều hành


---------------------------------------------------------------


Như mình đã đề cập từ phần trước, Node là nơi đặt và vận hành các container ứng dụng (chính xác thì các container được đặt trong các Pod, và Pod được đặt bên trong Node). Node được quản lý bởi **Control Plane**

Các thành phần chính trong một Node gồm có:

- **kubelet**: là phần mềm giúp đảm bảo việc các container sẽ được chay trong các Pod
- **kube-proxy**: quản lý các nguyên tắc truy cập mạng đến các Pod, từ bên trong hoặc bên ngoài cluster
- **Container runtime**: là phần mềm hỗ trợ việc chạy các container - một vd tiêu biểu là Docker

![node-overview](../images/Kubernetes/node-overview.svg)

*(Nguồn: kubernetes.io)*

## Quản lý Node trong Kubernetes

Có 2 cách để khai báo Node trong Kubernetes:

- **kubelet** của Node sẽ tự khai báo với API Server
- Người dùng tự tạo và thêm Node object

Vd, người dùng có thể tạo một Node bằng một khai báo JSON như sau:

```
{
  "kind": "Node",
  "apiVersion": "v1",
  "metadata": {
    "name": "10.240.79.157",
    "labels": {
      "name": "my-first-k8s-node"
    }
  }
}
```

Kubernetes sẽ kiểm tra để xem liệu một kubelet nào đó đã đăng ký tên Node này (metadata.name) với API Server hay chưa, nếu chưa thì việc khai báo Node mới mới được thực thi. Bạn cần lưu ý rằng, trong Kubernetes, tên Node là cách định danh của Node, vậy nên sẽ không thể có 2 Node với tên giống nhau nhé.

## Các thông tin về trạng thái của Node

Trạng thái của Node sẽ bao gồm những thông tin sau:
- Địa chỉ IP 
- Điều kiện
- Tài nguyên và khả năng phân bổ
- Thông tin chung 

Bạn có thể dùng trình dòng lệnh **kubectl** để kiểm tra các thông tin trạng thái của Node:

```
kubectl describe node <insert-node-name-here>
```

### Địa chỉ IP

Địa chỉ của Node bao gồm các trường cơ bản như sau:
- Hostname: là tên được khai báo bởi node
- ExternalIP: địa chỉ IP của node mà có thể được định tuyến từ bên ngoài của cluster
- InternalIP: địa chỉ IP của node mà chỉ có thể được định tuyến bên trong cluster

Ví dụ như ở đây trên máy tính Windows của mình, mình sử dụng lệnh sau để kiểm tra các node hiện tại trong cluster:

```
kubectl get nodes
```

Kết quả, kubectl trả về hiện tại mình chỉ có một node duy nhất (master) tên là **minikube**

!(nodes)[../images/Kubernetes/command/kubectl-get-nodes.PNG]


