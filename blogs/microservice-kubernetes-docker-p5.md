# Giới thiệu về Micro-service - Docker - Kubernetes (Phần 5)

Bắt đầu từ phần này, mình sẽ trình bày những phần đi sâu hơn về kiến trúc của Kubernetes, các thành phần trong một cụm Kubernetes và chức năng của chúng. Đương nhiên, để hiểu tường tận về cấu trúc bên dưới của một nền tảng phức tạp như K8S là một điều rất khó. Vậy nên, mình chỉ xin chia sẻ những hiểu biết của mình ở mức high-level, và hi vọng những kiến thức này sẽ giúp ích cho những người mới làm quen với K8S như mình hồi đó

Bắt đầu nhé !!!

## Kiến trúc tổng quan của một cụm K8S

Khi nói về việc triển khai Kubernetes, tức là chúng ta đang tạo ra một cụm Kubernetes (Kubernetes cluster). Một Kubernetes cluster hoàn chỉnh sẽ bao gồm nhiều máy tính thợ(worker machine), và một trung tâm điểu khiển - gọi là Control Plane. Mình sẽ giới thiệu kĩ hơn về các thành phần này ở phía sau, nhưng về cơ bản, kiến trúc này giống như một mô hình Master-Slaver thông thường trong đó:
- Các Worker machine trong cluster phụ trách việc chạy các ứng dụng được đóng gói (đóng vai trò Slaver)
- Control Plane phụ trách việc quản lý và điều hành toàn bộ cluster (đóng vai trò Master)

Bạn có thể tham khảo kiến trúc tổng quan này bên dưới

![kubernetes-component](../images/Kubernetes/components-of-kubernetes.svg)