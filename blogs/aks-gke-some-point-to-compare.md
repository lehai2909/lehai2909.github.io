# Một vài so sánh về K8S trên Azure (AKS) và Google Cloud (GKE)

## Security and Identiy

Kubernetes RBAC (role-based access control) là phương pháp chính để xác thực (authenticate) và cấp quyền (authorize) trong Kubernetes. Điều này thực hiện thông qua việc bạn tạo các vai trò (role) bên trong K8s, chỉ định các hành động (ví dụ như xem/tạo/chỉnh sửa tài nguyên như pod, secret,...) mà role này có thể thực hiện, và gán role này cho user, hoặc một nhóm user (group) xác định. Các quyền của role có thể ở quy mô namespace (đối với Role đơn thuần) hoặc ở quy mô toàn cluster (đối với ClusterRole) ([Tham khảo tài liệu K8S](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole))

Một thực thế đáng chú ý trong thành phần RBAC của K8S, là Service Account (SA). SA đuợc sử dụng chủ yếu bởi các process trong pod để tương tác với API Server, và SA là đối tượng ở mức namespace (các SA ở các namespace khác nhau có thể trung tên nhau)

Khác với SA, User Account (UA) được sử dụng để quản lý quyền cho một người cụ thể bên trong K8S (Như quản trị viên, hoặc lập trình viên). UA có quy mô global

### Identity trên Azure

Azure cũng có một hệ thống quản lý truy cập dựa trên cơ chê RBAC, và không có gì ngạc nhiên khi nó có tên là Azure RBAC :smiley:. Hệ thống này được xây dựng dựa trên [Azure Resource Manager](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview), và nó khá giống với các hệ thống IAM của các cloud provider khác. Về cơ bản, Azure RBAC cho phép bạn define các role, và gán các role này cho cho các user hoặc group thông qua role assignment, cho một scope nhất định. Về các scope cơ bản của Azure, bạn có thể xem hình bên dưới ([tham khảo tài liệu Azure](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview#understand-scope)):

![image](https://user-images.githubusercontent.com/49013652/227781999-82c4cc6d-e47c-4530-ab1e-d39d424fb2b4.png)

Ví dụ dưới đây, bạn gắn cho Alex một role tên là [Azure Kubernetes Service Contributor role](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#azure-kubernetes-service-contributor-role), một built-in role trong Azure. Bạn gắn role này lên Alex ở scope là một cluster cụ thể (một resource), vậy nên Alex sẽ có quyền truy cập để quản lý cluster này, nhưng không phải các cluster khác trong một resource group.

![Azure-RBAC](https://user-images.githubusercontent.com/49013652/227784387-8dce8988-15e3-43d9-8230-be05cb2f10b7.png)

Việc sử dụng kết hợp Azure RBAC và Kubernetes RBAC sẽ cho phép quản lý truy cập một cách toàn diện hơn. Một mặt, bạn sử dụng Azure RBAC để cấp quyền truy cập cho một đối tượng (user/group) đến AKS resource, thông qua việc gán role cho đối tượng này. Mặt khác, bạn sử dùng Kubernetes RBAC để quản lý quyền truy cập đến các tài nguyên bên trong cluster (pod, secret,...). Điều này khá giống với việc sử dụng kết hợp IAM và Kubernetes RBAC trong Google Cloud.

Chúng ta nói khá nhiều về việc quản lý truy cập đến các tài nguyên trên Azure hay K8S cho các đối tượng như User hoặc Group. Tuy nhiên, các đối tượng này được quản lý như thế nào trong Azure? Làm sao chúng ta có thể dễ dàng tạo một user trong Azure, gắn quyền cho đối tượng này, và liên kết đối tượng này với AKS?

AKS cho phép bạn tích hợp Azure Active Directory (Azure AD), giúp quản lý tập trung các user account/group, credentials, và quyền truy cập đến các tài nguyên trong Kubernetes. Workflow như sau:
![image](https://user-images.githubusercontent.com/49013652/227786252-3fed8d31-a0e0-48c6-a0ed-ae77f72c3007.png)

