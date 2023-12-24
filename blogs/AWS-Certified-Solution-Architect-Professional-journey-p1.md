# Chứng chỉ AWS Certified Solutions Architect - Professional (SAP-CO2) có gì?

![image](https://github.com/lehai2909/lehai2909.github.io/assets/49013652/5b02dbeb-c278-4b9d-99e1-d22c8db2f80f)

Chứng chỉ SAP-CO2 theo mình đánh giá  là một chứng chỉ tương đối khó. Bài thi đánh giá khả năng đưa ra giải pháp tối ưu khi thiết kế hệ thống trên AWS, và nội dung 65 câu hỏi trong bài thi yêu cầu bạn có một hiểu biết tương đối rộng về các dịch vụ trên AWS, và cần đưa ra một giải pháp tối ưu, hiệu quả và tiết kiệm đối với một vấn đề cụ thể. Sau đây là 4 domain mà bạn sẽ cần tập trung (Được lấy từ [exam guide](https://d1.awsstatic.com/training-and-certification/docs-sa-pro/AWS-Certified-Solutions-Architect-Professional_Exam-Guide.pdf)):

• [Domain 1: Design Solutions for Organizational Complexity (26% of scored
content)](#D1)

• Domain 2: Design for New Solutions (29% of scored content)

• Domain 3: Continuous Improvement for Existing Solutions (25% of scored
content)

• Domain 4: Accelerate Workload Migration and Modernization (20% of scored
content)

Để tránh việc overwhelm bằng cách liệt kê tất cả các sub domain, yêu cầu, dịch vụ,...blah...blah (bạn đọc hoàn toàn có thể đọc thêm chi tiết từ exam guide mình để ở trên), rút kinh nghiệm từ việc thảo luận về chứng chỉ Solution Architect Associate trước đây, ở trong bài này, mình sẽ cố gắng tóm gọn lại những phần quan trọng và các dịch vụ AWS cần lưu ý (theo ý kiến cá nhân của mình) để bạn có thể chuẩn bị tốt trước ngày đi thi.

**Lưu ý**: những thông tin trong bài này chỉ mang tính chất tham khảo, tác giả không chịu trách nhiệm cho bất kỳ việc thi trượt nào 👀 

## Domain 1: Design Solutions for Organizational Complexity {#d1}

### Những phần cần lưu ý:

1. Thiết kế kết nối mạng: 
- Một tổ chức lớn có hạ tầng chạy trên AWS có thể được thiết kế theo nhiều cách khác nhau: trên một hoặc nhiều VPC, trên nhiều VPC trên các region khác nhau, và đôi khi là kết nối từ cloud xuống hạ tầng chạy trên on-prem. Bạn cần nắm vững các khái niệm về network (VPC, subnet, route table,...), các dịch vụ giúp kết nối các vpc (hay giữa vpc và on-prem), các vấn đề về DNS, routing khi kết nối các mạng khác nhau thông qua gateway, và các dịch vụ giúp monitor/troubleshoot khi không thể kết nối...
- Các dịch vụ cần lưu ý:
  -  [VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html), VPC peering, Transit Gateway
  -  [Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html)
  -  [VPN](https://docs.aws.amazon.com/vpn/latest/s2svpn/how_it_works.html)
  -  []()
  -  []()
  -  []()
  -  []()

