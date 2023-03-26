# Một vài so sánh về K8S trên Azure (AKS) và Google Cloud (GKE)

## Security and Identiy

Kubernetes RBAC (role-based access control) là phương pháp chính để xác thực (authenticate) và cấp quyền (authorize) trong Kubernetes. Điều này thực hiện thông qua việc bạn tạo các vai trò (role) bên trong K8s, chỉ định các hành động (ví dụ như xem/tạo/chỉnh sửa tài nguyên như pod, secret,...) mà role này có thể thực hiện, và gán role này cho user, hoặc một nhóm user (group) xác định. Các quyền của role có thể ở quy mô namespace (đối với Role đơn thuần) hoặc ở quy mô toàn cluster (đối với ClusterRole) ([Tham khảo tài liệu K8S](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole))

Một thực thế đáng chú ý trong thành phần RBAC của K8S, là Service Account (SA). SA đuợc sử dụng chủ yếu bởi các process trong pod để tương tác với API Server, và SA là đối tượng ở mức namespace (các SA ở các namespace khác nhau có thể trung tên nhau)

Khác với SA, User Account (UA) được sử dụng để quản lý quyền cho một người cụ thể bên trong K8S (Như quản trị viên, hoặc lập trình viên). UA có quy mô global

### Identity trên Azure

Azure cũng có một hệ thống quản lý truy cập dựa trên cơ chê RBAC, và không có gì ngạc nhiên khi nó có tên là Azure RBAC :smiley:. Hệ thống này được xây dựng dựa trên [Azure Resource Manager](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview), và nó khá giống với các hệ thống IAM của các cloud provider khác. Về cơ bản, Azure RBAC cho phép bạn define các role, và gán các role này cho cho các user hoặc group thông qua role assignment, cho một scope nhất định. Về các scope cơ bản của Azure, bạn có thể xem hình bên dưới ([tham khảo tài liệu Azure](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview#understand-scope)):

![image](https://user-images.githubusercontent.com/49013652/227781999-82c4cc6d-e47c-4530-ab1e-d39d424fb2b4.png)

Ví dụ:
Bạn gán role Reader cho user lehai, ở scope là một AKS cluster 
