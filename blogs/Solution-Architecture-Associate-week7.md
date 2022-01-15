# Nhật ký tự học Solution Architecture Associate AWS - Week 7 (15/11/2021 - 21/11/2021)

![SAA-badge](../images/SAA/AWS-Certified_Solutions-Architect_Associate_badge.png)

## Những thứ mình đã học được trong một tuần qua:

### AWS CloudTrail

CloudTrail là dịch vu ghi lại những hành động của người dùng (user, role,...) hoặc của các dịch vụ AWS khác đã được thực hiện trong tài khoản. CloudTrail giúp kiểm soát những vấn đề như: tuân thủ, quản lý hoặc kiểm tra các hoạt động nguy hại

Các thành phần của CloudTrail:
+ Event: 1 bản ghi của hoạt động được thực hiện. Đươc phân làm 2 nhóm chính: Management Event và Data Event
+ Trail: là thiết lập cho phép chuyển giao các event tới S3 bucket hoặc CloudWatch Logs,... Sử dụng Trail để filter các event mong muốn

Sử dụng digest file để chứng thực tính xác thực của log trong S3

### AWS CloudWatch
CloudWatch cho phép giám sát các tài nguyên và ứng dụng trên AWS, thông qua việc thu thập và theo dõi các metrics
CloudWatch Logs cho phép thu thập log từ các ứng dụng khác như: cloudTrail logs, application log từ EC2 hoặc VPC flow logs
Các thành phần của CloudWatch logs:
+ log event
+ log stream
+ log group