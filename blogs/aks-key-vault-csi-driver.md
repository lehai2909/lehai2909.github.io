# Truy cập Azure Key Vault từ AKS với Secret Store CSI Driver
## Một vài khái niệm
CSI (Container Storage Interface) là cách thức K8S sử dụng để truy cập các ổ lưu trữ (dạng block, file,...) từ các nhà cung cấp bên ngoài, ví dụ các nhà cung cấp dịch vụ cloud như Azure hay Google Cloud. Hãy tạm gọi họ ở đây là các provider. Các provider này sẽ phát triển các CSI driver (thường dưới dạng open source) để hệ thống K8S có thể sử dụng để truy cập hệ thống lưu trữ của các provider này. Lấy một ví dụ, với [Azure File CSI driver](https://github.com/kubernetes-sigs/azurefile-csi-driver), bạn có thể mount và truy cập đến 
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

Secret Store CSI driver chạy dưới dạng daemonset trên mỗi node của K8S cluster. Các pod driver sẽ làm việc với kubelet để thiết lập việc kết nối đến Secret Store provider, cũng như mount/unmount data.

## Các bước để sử dụng Azure Key Vault Provider for Secrets Store CSI driver trong AKS
### 1. Cài đặt Azure Key Vault Provider for Secrets Store CSI Driver lên cluster:
Có 2 option có thể sử dụng ở đây:

#### Option 1: Kích hoạt `azure-keyvault-secrets-provider` add-on bằng az cli:

```az aks enable-addons --addons azure-keyvault-secrets-provider --name <cluster-name> --resource-group <resource-group>```

Sau đó, kiểm tra Azure Key Vault CSI driver đã được cài đặt trên cluster chưa:

```
kubectl get pods -n kube-system -l 'app in (secrets-store-csi-driver,secrets-store-provider-azure)'
```

Kết quả sẽ tương tự như sau, nếu việc enable add-on thành công:

```
NAME                                     READY   STATUS    RESTARTS   AGE
aks-secrets-store-csi-driver-4vpkj       3/3     Running   2          4m25s
aks-secrets-store-csi-driver-ctjq6       3/3     Running   2          4m21s
aks-secrets-store-csi-driver-tlvlq       3/3     Running   2          4m24s
aks-secrets-store-provider-azure-5p4nb   1/1     Running   0          4m21s
aks-secrets-store-provider-azure-6pqmv   1/1     Running   0          4m24s
aks-secrets-store-provider-azure-f5qlm   1/1     Running   0          4m25s
```

#### Option 2: Sử dùng Helm để cài đặt:

```
helm repo add csi-secrets-store-provider-azure https://azure.github.io/secrets-store-csi-driver-provider-azure/charts
helm install csi csi-secrets-store-provider-azure/csi-secrets-store-provider-azure --namespace kube-system
```

(Tham khảo: [https://azure.github.io/secrets-store-csi-driver-provider-azure/docs/getting-started/installation/](https://azure.github.io/secrets-store-csi-driver-provider-azure/docs/getting-started/installation/))

---------------------------------------------------------------------------------

### 2. Tạo Azure Key Vault (Lưu ý quan trọng!: nếu đã có sẵn thì không cần tạo thêm :hankey: )

Chúng ta có thể sử dụng console, hoặc az cli command [az keyvault create](https://learn.microsoft.com/en-us/cli/azure/keyvault#az-keyvault-create.md) để tạo:

```
az keyvault create -n <new-keyvault-name> -g <resource-group-name> -l <location>
```

Sử dụng [az keyvault secret set](https://learn.microsoft.com/en-us/cli/azure/keyvault#az-keyvault-secret-set.md) command để tạo một secret trong Key Vault ở trên, với nội dung secret là *ExampleSecretContent*

```
az keyvault secret set --vault-name <new-keyvault-name> -n <new-secret-name> --value ExampleSecretContent
```

### 3. Lựa chọn phương pháp định danh để truy cập Azure Key Vault

#### Option 1: Sử dụng Managed Identity

#### Option 2: Sử dụng Service Principal
