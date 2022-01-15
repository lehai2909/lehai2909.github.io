# Nhật ký tự học Solution Architecture Associate AWS - Week 1 (18/10/2021 - 24/10/2021)

![SAA-badge](../images/SAA/AWS-Certified_Solutions-Architect_Associate_badge.png)

## Những thứ mình đã học được trong một tuần qua:

### Lựa chọn IAM policy dựa trên phương thức truy cập

Dựa vào đối tượng cần truy cập tài nguyên, lựa chọn giữa **identity-based policy** và **resource-based policy**:
+ identity-based policy: gán policy này cho đối tượng cần cấp quyền truy cập: user, role or group. Cho phép xác định đối tượng hay nhóm đối tượng này có những quyền gì
+ resourced-based policy: gán policy này cho tài nguyên (ví dụ S3 bucket, KMS encryption keys,...). Cho phép xác định những đối tượng nào có thể sử dụng tài nguyên này

Cùng cần phân biệt giữa **managed-policy** và **inline-policy**:
+ managed-policy là policy được tạo và quản lý tập trung bởi AWS hoặc người dùng. Policy này được tạo ra và có thể được sử dụng nhiều lần cho nhiều đối tượng khác nhau
+ inline-policy là policy đươc gắn liền với một đối tượng, và chỉ có đối tượng đó được sử dụng policy này. Khi đối tượng này bị xóa đi, policy cũng đồng thời bị xóa. Điều này hữu ích cho việc bảo vệ policy, tránh cho policy bị gán nhầm cho đối tượng khác

### Mã hóa dữ liệu (Data Encryption)

Dự liệu có 2 trạng thái chính: **Data at rest** (những dữ liệu được nằm yên lưu trữ trên các phương tiện như EBS volumes hay S3) và **Data in transit** (những dữ liệu được truyền đi trên hệ thống network). Tùy vào trạng thái của dữ liệu, sẽ có những phương tiện mã hóa thích hợp

+ Data at rest:
Mã hóa dự liệu này tùy thuộc vào nơi đối tượng được lưu trữ (S3, EBS hay RDS). Các dịch vụ này được tích hợp với KMS  (Key Management Service)

S3 có những phương án mã hóa như sau: SSE - S3, SSE - KMS, SSE - C. 

+ Data in transit:
AWS Certificate Manager được sử dụng để tạo và quản lý chứng chỉ TLS (Transport Layer Security), đồng thời cài đặt chứng chỉ này trên ALB hoặc CloudFront

### 2nd Pillar: Kiến trúc bền vững (Resilient Architecture)

Một kiến trúc bền vững là kiến trúc có khả năng thực hiện chức năng một cách chính xác và nhất quán trong mọi hoàn cảnh. Kiến trúc cần có khả năng quản lý sự cố hay thay đổi có thể xảy ra, đồng thời có khả năng hồi phục nhanh chóng khi sự cố xảy ra

4 thành tố chính cần quan tâm:
+ Foundation (network infra)
+ Workload architecture
+ Change management
+ Failure management

### Elastic Load Balancing

ELB là dịch vụ cho phép phân bố lưu lượng truy cập một cách đồng đều đến những tài nguyên mục tiêu ở nhiều AZ trên AWS  (EC2 instance, container,ip address,...)

ELB gồm các **listener**. Listener là quá trình cho phép việc kiểm tra và thiết lập các yêu cầu kết nối. Listener duy trì 2 kết nối đồng thời: kết nối từ client đến Load balancer, và kết nối từ load balancer đến các target trong Target group. Các kết nối được config với protocol và port xác định (có thể khác nhau)

Trong listener, ta sử dụng rule để xác định cách load balancer sẽ điều chuyển luồng traffict đến các target như thế nào.

**Target group** chứa các target đã được đăng ký. Những target được xác định là health thông qua kiểm tra sẽ được phân bổ các traffic đầu vào. Việc xác định healthy target được thực hiện thông qua healthy check được config ở mỗi target group