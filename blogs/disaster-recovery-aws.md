# Phục hồi sau thảm họa trên AWS

## Một vài khái niệm

Một trong những yếu tố cần quan tâm đến khi xây dựng kiến trúc hệ thống trên AWS, là **Tính bền vững** **(Resiliency)**. Tính bền vững nhấn mạnh vào khả năng tự phục hồi của hệ thống khi xảy ra những gián đoạn về hạ tầng, dịch vụ, ứng dụng, làm ảnh hưởng tới người dùng. Ví dụ, một ứng web có tính bền vững, sẽ có khả năng tự phục hồi khi có sự cố về hạ tầng mạng, ví dụ đứt cáp mạng, khiến cho các data center ở khu vực (region trong AWS) mà website này được host bị mất kết nối. Khi đó, người dùng vẫn có thể truy cập đến trang web này, nếu các biện pháp phục hồi đã được lên kế hoạch và triển khai tốt từ trước.

Phục hồi sau thảm họa (**DR - Disater Recovery**) là một trong những biện pháp như thế. DR liên quan đến việc hệ thống của bạn phản ứng như thé nào khi có thảm họa xảy ra ở một khu vực hoặc nhiều khu vực, ví dụ, thiên tai như động đất, sóng thần, hoặc nhưng thảm họa xảy ra do sự cố kỹ thuật, như mất nguồn điện.

Việc triển khai DR phải dựa trên các tiêu chí phục hồi của tổ chức. 2 tiêu chí rất phổ biến hiện nay dùng trong việc đánh giá khả năng phục hồi của một hệ thống, là **RTO (Recovery Time Objective)** và **RPO (Recovery Point Objective)** 

![image](https://github.com/lehai2909/lehai2909.github.io/assets/49013652/d1016445-cd14-4ea6-ba96-cf7ae3ee30c9)

_(Nguồn: Disaster Recovery of Workloads on AWS: Recovery in the Cloud - AWS Whitepaper)_

Trong hình trên, RTO là điểm nằm bên phải sau khi thảm họa xảy ra. RTO cho biết khoảng thời gian tối đa chấp nhận được để hệ thống có thể phục hồi sau khi thảm họa xảy ra. 

RPO, ngược lại là điểm nằm phía bên trái, trước thời điểm thảm họa xảy ra. Nó cho biết khoảng thời gian tối đa, kể từ lần cuối dữ liêu được sao lưu và bảo vệ, cho đến khi thảm họa xảy ra. Trong khoảng thời gian đó, dữ liệu coi như bị thất thoát, và RPO cho ta biết được lượng dữ liệu bị thất thoát tối đa có thể chấp nhận được.

Các tiêu chí về RTO và RPO sẽ quyết định cách chúng ta lựa chọn các chiến lược DR phù hợp.

## Các chiến lược DR

Bên dưới là tổng quan về 4 chiến lược DR, được sắp xếp từ trái sang phải theo chiều độ phức tạp và chi phí tăng dần:

![image](https://github.com/lehai2909/lehai2909.github.io/assets/49013652/8397269d-ba96-4b75-9d70-dcffd6d05919)

_(Nguồn: Disaster Recovery of Workloads on AWS: Recovery in the Cloud - AWS Whitepaper)_
