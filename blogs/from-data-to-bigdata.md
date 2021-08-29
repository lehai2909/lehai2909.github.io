# Cách chúng ta lưu trữ dữ liệu đã thay đổi như thế nào?

## Từ Hệ quản trị cơ sở dữ liệu đến Data Lake

Có rất nhiều thứ đã diễn ra trong hàng chục năm qua: sự bùng nổ của dữ liệu lớn, sự ra đời của các nền tảng tính toán phân tán như Hadoop, Spark để đáp ứng (dù chỉ là một phần) nhu cầu tính toán trên dữ liệu lớn, với mục đích cuối cùng theo mình được tóm tắt trong một biểu thức đơn giản: **Data => Information**

Bên cạnh khả năng tính toán, một thứ khác luôn luôn song hành và phải được nhắc tới: khả năng lưu trữ. Qua từng giai đoạn, lại có những mô hình lưu trữ mới được tạo ra, nhằm bù đắp những thiếu sót của các mô hình cũ, hay để đáp ứng yêu cầu lưu trữ ngày càng nhanh và đa dạng của dữ liệu lớn. 2 ví dụ điển hình mà chúng ta có thể nêu ra:

- Sự chuyển đổi từ việc sử dụng các spreadsheet (bảng tính) như Excel đến sự ra đời và bùng nổ của các hệ cơ sở dữ liệu quan hệ (RDBMS) như Oracle Database hay MySQL, theo sau bằng sự ra đời của các hệ cơ sở dữ liệu NoSQL
- Sự xuất hiện của khái niệm Data Lake dần dần thay thế mô hình Data Warehouse truyền thống

Nếu bạn cảm thấy xa lạ và hốt hoảng với những khái niệm này, đừng lo và hãy kiên nhẫn :smile:. Trong loạt bài tới, mình sẽ cố gắng trình bày một cách tổng quan và dễ hiểu nhất về những vấn đề đã nêu trên đây

### Hệ quản trị cơ sở dữ liệu quan hệ

Hệ quản trị cơ sở dữ liệu quan hệ (Relational Database Management System - RDBMS) là hệ thống cho phép người dùng mô tả dữ liệu dưới dạng các bảng (table), trong đó mỗi bảng sẽ có một số trường dữ liệu (field), được thể hiện dưới dạng các cột. Mỗi bảng thường dùng để lưu thông tin về các đối tượng trong một thực thể - ví dụ: các nhân viên trong một công ty, hay các giao dịch của một cửa hàng. Mỗi cột, hay mỗi trường dữ liệu như đã đề cập, chứa đựng một thuộc tính (attribute) của đối tượng

Hãy lấy một ví dụ để đơn giản hóa những khái niệm vừa nêu. Giả sử một trường  đại học cần lưu trữ thông tin của toàn bộ sinh viên. Họ có thể sử dụng RDBMS để lưu trữ dữ liệu của sinh viên dưới dạng bảng như sau (hãy chú ý những trường dữ liệu được tô đậm):


| ID      | Tên | Ngày/tháng/năm sinh | Quê quán | Khoa | Trưởng khoa |  
| ----------- | ----------- | ---------|----------|--------|----------|
| BK001      | Lê Hải       |21/07/1996 | Đà Nẵng | PFIEV | Lê Cung |
| BK002      | Nguyễn Hoàng    |22/06/1992 | Đà Nẵng | PFIEV | Lê Cung |
| BK003      | Nguyễn Lan Anh    |10/01/1996 | Quảng Bình | PFIEV | Lê Cung |
| BK004      | Ngô Phương Nhi   |17/08/1995 | Huế | ECE | Nguyễn Lê Hòa |
| ...      | ...   |...| ... | ... | ... |


Bảng trên nêu rất đầy đủ các thông tin về mỗi sinh viên, từ mã số các nhân, họ tên, v...v đến thông tin về khoa theo học và Trưởng của khoa đó. Mỗi hàng trong bảng trên (ngoại trừ hàng đầu tiên để chỉ ra tên của trường thông tin) được gọi là một bảng ghi (record). Mỗi bảng ghi chứa thông tin về một đối tượng (ở đây là một sinh viên), còn mỗi cột chứa một mẩu thông tin về đối tượng đó

Và bây giờ, bạn quyết định chỉ dùng một bảng duy nhất như trên để lưu trữ tất cả thông tin. Điều đó thật tuyệt khi bạn chỉ muốn truy cập (read) thông tin này: Tất cả thông tin đều chỉ tập trung duy nhất ở một nơi (một bảng), và bạn không cần truy cập đến những bảng khác. Thế nhưng, chúng ta hãy xem xét kĩ lại bảng thông tin này một lần nữa:

| ID      | Tên | Ngày/tháng/năm sinh | Quê quán | Khoa | Trưởng khoa |  
| ----------- | ----------- | ---------|----------|--------|----------|
| BK001      | Lê Hải       |21/07/1996 | Đà Nẵng | **PFIEV** | **Lê Cung** |
| BK002      | Nguyễn Hoàng    |22/06/1992 | Đà Nẵng | **PFIEV** | **Lê Cung** |
| BK003      | Nguyễn Lan Anh    |10/01/1996 | Quảng Bình | **PFIEV** | **Lê Cung** |
| BK004      | Ngô Phương Nhi   |17/08/1995 | Huế | ECE | Nguyễn Lê Hòa |
| ...      | ...   |...| ... | ... | ... |

Nếu để ý kỹ, bạn sẽ thấy rất nhiều trường thông tin bị lặp đi lặp lại (thông tin về khoa và trưởng khoa)
