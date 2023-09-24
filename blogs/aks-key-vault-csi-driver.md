# Truy cập Azure Key Vault từ AKS với Secret Store CSI Driver
## Một vài khái niệm
CSI (Container Storage Interface) là cách thức chúng cung cấp các ổ lưu trữ (dạng block, file,...) từ các nhà cung cấp bên ngoài, chẳng hạn như các nhà cung cấp dịch vụ cloud như Azure hay Google Cloud. Hãy tạm gọi họ ở đây là các provider. Các provider này sẽ phát triển các CSI driver (thường dưới dạng open source) để hệ thống K8S có thể sử dụng để truy cập hệ thống lưu trữ của các provider này, ví dụ như Google Persistent Disk của GCP.
