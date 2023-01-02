# Nhật ký tự học Solution Architecture Professional AWS - Week 1 

![image](https://user-images.githubusercontent.com/49013652/210194181-70e29232-8393-41c8-ba05-5c9cd5524568.png)

## Domain 1: Design Solutions for Organizational Complexity

### Connectivity oftion for multiple VPC

#### VPC Peering

![image](https://user-images.githubusercontent.com/49013652/210194283-00684296-937e-4e71-a5b3-ef9ce0f1dc54.png)

VPC Peering cho phép việc kết nối giữa các VPC sử dụng địa chỉ IP private
VPC Peering có thể đuợc thiết lập giữa VPC trong một account, hoặc của 2 account khác nhau, hỗ trơ inter-region peering
VPC Peering là kết nối 1-1, nên khi môi trường mở rộng và có thêm nhiều VPC mới, VPC peering sẽ không scale tốt


#### Transit Gateway

Transit Gateway là một lựa chọn tốt hơn để kết nối nhiều VPC theo kiến trúc hub-và-spoke. Mỗi spoke VPC chỉ cần kết nối đến Transit Gateway để có thể kết nối đến các VPC khác

![image](https://user-images.githubusercontent.com/49013652/210195517-74e6d961-8637-46c7-bbeb-23721c9a9c2d.png)

Transit Gateway cũng có thể được peer với nhau để kết nối VPC kiểu inter-region:

![image](https://user-images.githubusercontent.com/49013652/210195565-f1e33834-4811-450b-9720-c0758d26018d.png)

#### Private Link
