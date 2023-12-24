# Chứng chỉ AWS Certified Solutions Architect - Professional (SAP-CO2) có gì?

![image](https://github.com/lehai2909/lehai2909.github.io/assets/49013652/5b02dbeb-c278-4b9d-99e1-d22c8db2f80f)

Chứng chỉ SAP-CO2 theo mình đánh giá  là một chứng chỉ tương đối khó. Bài thi đánh giá khả năng đưa ra giải pháp tối ưu khi thiết kế hệ thống trên AWS, và nội dung 65 câu hỏi trong bài thi yêu cầu bạn có một hiểu biết tương đối rộng về các dịch vụ trên AWS, và cần đưa ra một giải pháp tối ưu, hiệu quả và tiết kiệm đối với một vấn đề cụ thể. Sau đây là 4 domain mà bạn sẽ cần tập trung (Được lấy từ [exam guide](https://d1.awsstatic.com/training-and-certification/docs-sa-pro/AWS-Certified-Solutions-Architect-Professional_Exam-Guide.pdf)):

• Domain 1: Design Solutions for Organizational Complexity (26% of scored
content)

• Domain 2: Design for New Solutions (29% of scored content)

• Domain 3: Continuous Improvement for Existing Solutions (25% of scored
content)

• Domain 4: Accelerate Workload Migration and Modernization (20% of scored
content)

Để tránh việc overwhelm bằng cách liệt kê tất cả các sub domain, yêu cầu, dịch vụ,...blah...blah (bạn đọc hoàn toàn có thể đọc thêm chi tiết từ exam guide mình để ở trên), rút kinh nghiệm từ việc thảo luận về chứng chỉ Solution Architect Associate trước đây, ở trong bài này, mình sẽ cố gắng tóm gọn lại những phần quan trọng và các dịch vụ AWS cần lưu ý (theo ý kiến cá nhân của mình) để bạn có thể chuẩn bị tốt trước ngày đi thi.

**Lưu ý**: những thông tin trong bài này chỉ mang tính chất tham khảo, tác giả không chịu trách nhiệm cho bất kỳ việc thi trượt nào 👀 

## Domain 1: Design Solutions for Organizational Complexity

### Những phần cần lưu ý:



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
