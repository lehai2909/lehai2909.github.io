# Giới thiệu về Micro-service - Docker - Kubernetes (Phần 2)

Tiếp nối phần 1, mình sẽ tiếp tục nói về Công nghệ Container trong bài này nhé...

Cuối bài trước, mình đã giới thiệu sơ qua về Container, nó là gì và tại sao người ta cần nó. Container hỗ trợ việc đóng gói ứng dụng và các thành phần liên quan thành một khối duy nhất tách biệt với cơ sở hạ tầng bên dưới, điều đó khiến cho một khối container có thể được điều chuyển dễ dàng giữa môi trường phát triển và môi trường triển khai. Do vậy, Container cho phép việc triển khai CI/CD (Continuous Integrate/Continuous Delivery) được nhanh chóng, khi mà các phần mềm hiện đại ngày càng cần được tích hợp và cập nhật một cách liên tục và nhanh chóng.

Về mặt kĩ thuật, nhiều container có thể cùng tồn tại song song trong cùng một hệ điều hành của máy tính, vì chúng là các quá trình (process) hoàn toàn tách biệt đã được đóng gói. Điều này cũng giống như nhiều container nằm riêng biệt bên trong một bến cảng vậy.

Trước khi công nghệ Container ra đời, người ta sử dụng máy ảo (Virtual Machine). Giống như container, máy ảo là một công nghệ cho phép việc tách biệt hóa các ứng dụng trong một môi trường riêng biệt. Điều đáng chú ý là, máy ảo sẽ trông giống như là một *máy tính riêng biệt* được chạy bên trong máy chủ của bạn. Mỗi máy ảo sẽ có hệ điều hành riêng, ổ cứng với dung lượng riêng (mặc dù điều này không thực sự đúng: dung lượng này là một phần dung lượng ổ cứng của bạn được tách ra để phục vụ việc chạy máy ảo), và cả những ứng dụng riêng của nó nữa. Đó là lí do tại sao mình lại nói nó dường như là một máy tính độc lập bên trong máy của bạn vậy. Điều đó cũng có nghĩa là bạn sẽ tốn đến hàng GB dung lượng để tạo nên môi trường cho một máy ảo có thể vận hành. Nếu cần chạy nhiều máy ảo cùng lúc bạn sẽ cần đến một chương trình giám sát (hypervisor)

Khác với máy ảo, công nghệ container cho phép cùng chia sẻ hệ điều hành bên dưới của máy tính, trong khi mỗi container, như đã nói, vẫn là một quá trình tách biệt. Để làm được điều này, Container đã được xây dựng dựa trên 2 khái niệm của hệ điều hành Linux: **cgroups** và **namespace** (mình sẽ trình bày kĩ hơn trong những bài tiếp theo). Điều này có nghĩa là việc khởi chạy một container sẽ chiếm ít không gian vật lý hơn rất nhiều so với một máy ảo (khoảng vài chục đến vài trăm MB), và thời gian khởi tạo (boot) cũng nhanh hơn đáng kể đấy. Việc vận hành các container sẽ do những môi trường như Docker đảm nhiệm.

Nhiều chữ như vậy thì khó hiểu quá nhỉ. Bạn có thể xem hình dưới đây để có cái nhìn tổng quát hơn về Container và Máy ảo nhé:


<p float="left">
  <img src="../images/virtual-machine.png" width="400" />
  <img src="../images/container.png" width="400" /> 
</p>

*(Nguồn: https://www.docker.com/resources/what-container)*

*(Còn tiếp...)








