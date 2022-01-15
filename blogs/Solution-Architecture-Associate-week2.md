# Nhật ký tự học Solution Architecture Associate AWS - Week 1 (11/10/2021 - 17/10/2021)

![SAA-badge](../images/SAA/AWS-Certified_Solutions-Architect_Associate_badge.png)

## Những thứ mình đã học được trong một tuần qua:

### Elastic Cloud Compute (EC2):
Là sức mạnh tính toán chính trên AWS, EC2 bao gồm những instance được phân chia theo mục đích chính khác nhau:
+ General Purpose
+ Compute Optimized
+ Memory Optimized
+ Storage Optimized
+ Accelerated Computing

Các loại instance này khác nhau về các khía cạnh:
+ Số CPU ảo (vCPU)
+ Storage
+ Memory
+ Networking

Các lựa chọn về giá cho EC2:
+ On - demand instance: dùng bao nhiêu trả bấy nhiêu, theo giờ. Người dùng không cần cam kết trước với AWS
+ Reserved instance: Người dùng xác định một cam kết sử dụng loại instance cố định trong khoảng thời gian 1 - 3 năm, với loại instance và Region xác định trước
+ Spot instance: yêu cầu sức mạnh tính toán dư của EC2, đỡ tốn kém tới 95% so với on-demand. Nhưng AWS có quyền thu hồi lại khi yêu cầu
+ Dedicated Instance: đặt các instance ở những server vật lý tách biệt với các account khác
+ Dedicated Host: cho bạn toàn quyền quyết định với server vật lý của mình, như đặt instance ở đâu, license nào.

### Elastic Block Storage (EBS):

Các loại EBS chính:
+ SSD EBS: For I/O optimized
+ HDD EBS For throughput optimized

### Phát hiện:
+ Sử dụng các công cụ logging: AWS CloudTrail (lưu lại lịch sử các hoạt động tài khoản), CloudWatch, VPC Flow log (lưu lại các lưu lượng ra vào mạng VPC)
+ phát hiện các sự cố config các resource trên AWS: AWS Config
### Phát hiện các hành vi bất thường
+ AWS GuardDuty (giám sát và phát hiện các mối đe dọa)
+ AWS WAF: giám sác các yêu cầu HTTP bất thường đến CloudFront, API Gateway,...
+ AWS Shield: bảo vệ ứng dụng khỏi các cuộc tấn công (DDos)

Để bảo về cơ sở hạ tầng Network, những việc có thể làm:
+ phân chia các layer (subnet, public or private?), đặt những group ứng dụng vào các lớp tương ứng (application layer, database layer,...)
+ Sử dụng các lớp bảo mật khác nhau (NACL, Security group, firewall (WAF)..)


