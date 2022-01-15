# Nhật ký tự học Solution Architecture Associate AWS - Week 6 (8/11/2021 - 14/11/2021)

![SAA-badge](../images/SAA/AWS-Certified_Solutions-Architect_Associate_badge.png)

## Những thứ mình đã học được trong một tuần qua:

### AWS Route 53 DNS

Dịch vụ DNS được cung cấp bởi AWS, gồm 3 chức năng chính sau đây:
+ Đăng ký tên miền
+ Kiểm tra sức khỏe tài nguyên (resource health checking (web server,...))
+ Điều phối lưu lượng đến tài nguyên thông qua tên miền trên internet

Từ hosted zone đã đăng ký với AWS, người dùng có thể route traffic đến các dịch vụ khác như: website tĩnh trên S3, Cloud distribution, ELB,....

Route 53 sử dụng các routing policies nhằm phân phối lưu lượng đến tài nguyên một cách hợp lý. Các policy bao gồm:
+ weighted routing
+ latency routing
+ failover routing
+ geolocation routing

### AWS ElatiCache
Elastic Cache là môt In-memory database được dùng để cache dữ liệu từ các cơ sở dữ liệu khác để cải thiện hiệu năng của ứng dụng thông qua việc cải thiện hiệu năng việc truy vấn dữ liệu

Terminology:
+ node: đơn vị nhỏ nhất của ElastiCache. Là một instance chạy cache engine
+ shard: nhóm các node, bao gồm 1 primary (read/write) node và các replicate (read-only) node được sao chép không đồng bộ từ primary node 
+ cluster: nhóm các shard, data được phân vùng (partition) trên các shard

### AWS CloudFront

CloudFront là dịch vụ cung cấp các các edge location trên khắp thế giới, cho phép đưa nội dung đến người dùng một cách nhanh nhất thông qua việc caching dữ liệu tại những địa điểm gần người sử dụng.
Khi người dùng yêu cầu một nội dung từ CloudFront, yêu cầu sẽ được chuyển tới edge location gần nhất:
+ nếu nội dung đã được cache tại edge location, nội dung sẽ được trả về cho người dùng ngay lập tứ
+ nếu nội dung không có sẵn, CF sẽ yêu cầu nó từ nguồn mà người dùng định sẵn, như S3 bucket hoặc web server, EC2 instance,...


