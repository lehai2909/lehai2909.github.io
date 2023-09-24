# Truy cập Azure Key Vault từ AKS với Secret Store CSI Driver
## Một vài khái niệm
CSI (Container Storage Interface) là cách thức K8S sử dụng để truy cấp các ổ lưu trữ (dạng block, file,...) từ các nhà cung cấp bên ngoài, ví dụ các nhà cung cấp dịch vụ cloud như Azure hay Google Cloud. Hãy tạm gọi họ ở đây là các provider. Các provider này sẽ phát triển các CSI driver (thường dưới dạng open source) để hệ thống K8S có thể sử dụng để truy cập hệ thống lưu trữ của các provider này. Lấy một ví dụ, với [Azure File CSI driver](https://github.com/kubernetes-sigs/azurefile-csi-driver), bạn có thể mount và truy cập đến 
Azure File dưới dạng volume trong pod.

Tuy nhiên, trong nhiều trường hợp, thứ ta muốn truy cập không phải là một volume lưu trữ, mà là một số thông tin bảo mật như secret, key hay certificate. Để làm được điều đó, provider cần phát triển CSI driver, cho phép tương tác với nơi lưu trữ bảo mật (thường được gọi là Secret Store), và mount secret/key muốn sử dụng vào pod dưới dạng volume.

Azure Key Vault là Secret Store chính của Azure, giúp giải quyết 3 vấn đề chính:
- Quản lý Secret: vd, Token hay password để kết nối đến DB
- Quản lý Key: vd, encryption key để mã hoá dữ liệu
- Quản lý Certificate: vd, TLS certificate

Để tích hợp Azure Key Vault vào Azure Kubernetes Service (AKS), chúng ta có thể sử dụng [Azure Key Vault Provider for Secrets Store CSI Driver](https://azure.github.io/secrets-store-csi-driver-provider-azure/docs/getting-started/)

## Nó hoạt động như thế nào?
Biểu đồ bên dưới mô tả cách thức hoạt động của Secret Store CSI driver nói chung (Nguồn: [https://secrets-store-csi-driver.sigs.k8s.io/concepts.html](https://secrets-store-csi-driver.sigs.k8s.io/concepts.html)):
![image](https://github.com/lehai2909/lehai2909.github.io/assets/49013652/f8580487-cd85-41a5-8916-9e98b08a74d8)

Secret Store CSI driver chạy dưới dạng daemonset trên mỗi node của K8S cluster. 
