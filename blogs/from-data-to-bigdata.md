# Cách chúng ta lưu trữ dữ liệu đã thay đổi như thế nào?

## Từ Hệ quản trị cơ sở dữ liệu đến Data Lake

Có rất nhiều thứ đã diễn ra trong hàng chục năm qua: sự bùng nổ của dữ liệu lớn, sự ra đời của các nền tảng tính toán phân tán như Hadoop, Spark để đáp ứng (dù chỉ là một phần) nhu cầu tính toán trên dữ liệu lớn, với mục đích cuối cùng theo mình được tóm tắt trong một biểu thức đơn giản: **Data => Information**

Bên cạnh khả năng tính toán, một thứ khác luôn luôn song hành và phải được nhắc tới: khả năng lưu trữ. Qua từng giai đoạn, lại có những mô hình lưu trữ mới được tạo ra, nhằm bù đắp những thiếu sót của các mô hình cũ, hay để đáp ứng yêu cầu lưu trữ ngày càng nhanh và đa dạng của dữ liệu lớn. 2 ví dụ điển hình mà chúng ta có thể nêu ra:

- Sự chuyển đổi từ việc sử dụng các spreadsheet (bảng tính) như Excel đến sự ra đời và bùng nổ của các hệ cơ sở dữ liệu quan hệ (RDBMS) như Oracle Database hay MySQL, theo sau bằng sự ra đời của các hệ cơ sở dữ liệu NoSQL
- Sự xuất hiện của khái niệm Data Lake dần dần thay thế mô hình Data Warehouse truyền thống

Nếu bạn cảm thấy xa lạ và hốt hoảng với những khái niệm này, đừng lo và hãy kiên nhẫn :smile:. Trong loạt bài tới, mình sẽ cố gắng trình bày một cách tổng quan và dễ hiểu nhất về những vấn đề đã nêu trên đây

### Hệ quản trị cơ sở dữ liệu quan hệ

Hệ quản trị cơ sở dữ liệu quan hệ (Relational Database Management System - RDBMS) là hệ thống cho phép người dùng mô tả dữ liệu dưới dạng các bảng (table), trong đó mỗi bảng sẽ có một số trường dữ liệu (field), được thể hiện dưới dạng các cột. Mỗi bảng thường dùng để lưu thông tin về các đối tượng trong một thực thể - ví dụ: các nhân viên trong một công ty, hay các giao dịch của một cửa hàng. Mỗi cột, hay mỗi trường dữ liệu như đã đề cập, chứa đựng một thuộc tính (attribute) của đối tượng

Hãy lấy một ví dụ để đơn giản hóa những khái niệm vừa nêu. Giả sử một trường  đại học cần lưu trữ thông tin của toàn bộ sinh viên. Họ có thể sử dụng RDBMS để lưu trữ dữ liệu của sinh viên dưới dạng bảng như sau:


| ID      | Tên | Ngày/tháng/năm sinh | Quê quán | Khoa | Trưởng khoa |  
| ----------- | ----------- | ---------|----------|--------|----------|
| BK001      | Lê Hải       |21/07/1996 | Đà Nẵng | PFIEV | Lê Cung |
| BK002      | Nguyễn Hoàng    |22/06/1992 | Đà Nẵng | PFIEV | Lê Cung |
| BK003      | Nguyễn Lan Anh    |10/01/1996 | Quảng Bình | PFIEV | Lê Cung |
| BK004      | Ngô Phương Nhi   |17/08/1995 | Huế | ECE | Nguyễn Lê Hòa |
| ...      | ...   |...| ... | ... | ... |


Bảng trên nêu rất đầy đủ các thông tin về mỗi sinh viên, từ mã số các nhân, họ tên, v...v đến thông tin về khoa theo học và Trưởng của khoa đó. Mỗi hàng trong bảng trên (ngoại trừ hàng đầu tiên để chỉ ra tên của trường thông tin) được gọi là một bảng ghi (record). Mỗi bảng ghi chứa thông tin về một đối tượng (ở đây là một sinh viên), còn mỗi cột chứa một mẩu thông tin về đối tượng đó

Và bây giờ, bạn quyết định chỉ dùng một bảng duy nhất như trên để lưu trữ tất cả thông tin. Điều đó thật tuyệt khi bạn chỉ muốn truy cập (read) thông tin này: Tất cả thông tin đều chỉ tập trung duy nhất ở một nơi (một bảng), và bạn không cần truy cập đến những bảng khác. Thế nhưng, chúng ta hãy xem xét kĩ lại bảng thông tin này một lần nữa (chú ý phần tô đậm):

| ID      | Tên | Ngày/tháng/năm sinh | Quê quán | Khoa | Trưởng khoa |  
| ----------- | ----------- | ---------|----------|--------|----------|
| BK001      | Lê Hải       |21/07/1996 | Đà Nẵng | **PFIEV** | **Lê Cung** |
| BK002      | Nguyễn Hoàng    |22/06/1992 | Đà Nẵng | **PFIEV** | **Lê Cung** |
| BK003      | Nguyễn Lan Anh    |10/01/1996 | Quảng Bình | **PFIEV** | **Lê Cung** |
| BK004      | Ngô Phương Nhi   |17/08/1995 | Huế | ECE | Nguyễn Lê Hòa |
| ...      | ...   |...| ... | ... | ... |

Nếu để ý kỹ, bạn sẽ thấy rất nhiều trường thông tin bị lặp đi lặp lại (thông tin về khoa và trưởng khoa). Điều này khiến cho việc lưu trữ thông tin chiếm nhiều bộ nhớ hơn với những dữ liêu dư thừa (redundancy), mà quan trọng hơn, khi có sự thay đổi về thông tin liên quan đến những dữ liệu dư thừa này, việc cập nhật dữ liệu (write data) sẽ tốn nhiều thời gian hơn, vì chúng ta sẽ phải sửa lại cùng một trường thông tin ở nhiều nơi khác nhau trong bảng

Giải pháp cho vấn đề này? Một kỹ thuật được áp dụng rộng rãi được gọi là **Normalization** (Chuẩn hóa). Nguyên tắc của kỹ thuật này khá đơn giản và dễ hiểu: thay vì quản lý tập trung quá nhiều thông tin trong 1 bảng như vậy, chúng ta sẽ tách chúng ra thành các bảng nhỏ hơn, mỗi bảng sẽ lưu trữ thông tin về một đối tượng rõ ràng hơn. Việc "chia để trị" như vậy giúp cho việc quản lý thông tin trở nên dễ dàng hơn, đồng thời hạn chế các thông tin dư thừa, như các bạn sẽ thấy ngay sau đây.

Để áp dụng kỹ thuật Normalization với bảng thông tin sinh viên ở trên, mình sẽ chia nhỏ nó ra thành 2 bảng: 1 bảng chứa thông tin riêng của sinh viên, bảng còn lại chứa thông tin về các khoa của trường. Cấu trúc của từng bảng sẽ như sau:


**Bảng 1 chứa thông tin riêng của sinh viên**:

| ID      | Tên | Ngày/tháng/năm sinh | Quê quán | Mã số khoa|
| ----------- | ----------- | ---------|----------|--------|
| BK001      | Lê Hải       |21/07/1996 | Đà Nẵng | 003|
| BK002      | Nguyễn Hoàng    |22/06/1992 | Đà Nẵng | 003|
| BK003      | Nguyễn Lan Anh    |10/01/1996 | Quảng Bình | 003 |
| BK004      | Ngô Phương Nhi   |17/08/1995 | Huế | 005 | 
| ...      | ...   |...| ... | ... |


**Bảng 2 chứa thông tin về các khoa của trường**:

|Mã số Khoa| Tên khoa | Trưởng khoa|
| ----------- | ----------- | ---------|
|001|Điện| Lê Hùng|
|002|Công nghệ thông tin| Phan Thanh Hưng|
|003|PFIEV| Lê Cung|
|004|Tự động hóa| Diệp Vấn|
|005|ECE| Nguyễn Lê Hòa|
|...|...|...|


Bây giờ, thông tin về một khoa chỉ cần lưu trữ một lần trong bảng 1, chứ không phải bị lặp lại tại nhiều nơi như trong cách lưu trữ trước kia. Tuy nhiên, có vẻ như chúng ta có một vấn đề mới: nếu thông tin về sinh viên và thông tin về khoa nằm ở 2 nơi khác nhau, làm sao chúng ta có thể tìm thông tin về khoa một sinh viên đang theo học, chỉ dựa và ID của sinh viên đó?

Đầu tiên, mình muốn các bạn để ý là mình đã thêm cột *Mã số khoa* vào 2 bảng mới tạo. Cột Mã số khoa trong Bảng 2 được dùng để gán cho mỗi khoa một số định danh duy nhất. Điều này là cần thiết vì mỗi khoa là một đối tượng riêng biệt duy nhất. Cũng giống như mỗi người chúng ta đều có một số CMND riêng duy nhất được đăng ký để phân biệt mỗi chúng ta với các cá nhân khác trong xã hội. Trong SQL (ngôn ngữ thông dụng để truy vấn dữ liêu dạng bảng), côt này được gọi là **Primary Key**. Các giá trị trong cột này, như đã được nhấn mạnh, là hoàn toàn duy nhất và đại diện cho mỗi record trong bảng.

Cột Mã số khoa trong Bảng 1 tương đối khác với thứ chúng ta vừa nêu ở trên. Mục đích của cột này không phải dùng để định danh, mà là để tham chiếu tới cột Mã số khoa trong Bảng 2. Tham chiếu ở đây, có nghĩa là khi chúng ta nhìn thấy một sinh viên trong Bảng 1 có giá trị tại cột Mã số khoa là 003, chúng ta sẽ tham chiếu sang Bảng 2, tìm record có giá trị tại cột Mã số khoa là 003, từ đó chúng ta sẽ biết rằng, khoa với mã số 003 có tên là PFIEV, do thầy Lê Cung là trưởng khoa. Trong ngôn ngữ SQL, cột Mã số khoa trong bảng 1 được gọi là **Foreign Key**. Foreign Key chỉ được dùng để tham chiếu chứ không phải để định danh, nên các giá trị tại cột này có thể trùng lặp tại nhiều record trong Bảng 1, biểu thị rằng các sinh viên tại các record này là thuộc cùng một khoa.

Việc tham chiếu dữ liệu như mình vừa làm là hoàn toàn thủ công. Tuy nhiên, trong các hệ thống RDBMS lớn với hàng triệu record dữ liệu trong mỗi bảng, việc đó là hoàn toàn không khả thi. Thay vào đó, các hệ thống này sẽ hỗ trợ cú pháp **join** trong ngôn ngữ SQL, cho phép việc tham chiếu thông tin giữa các bảng như trên được diễn ra tự động và nhanh chóng. Do bài viết không nhằm mục đích trình bày về ngôn ngữ SQL, nên mình sẽ không đi sâu vào vấn đề này. W3School cung cấp một [tutorial](https://www.w3schools.com/sql/default.Asp) rất hay và hiệu quả về SQL cũng như join dành cho những ban nào quan tâm nhé

*Lời kết*: Trong bài này, mình đã trình bày tổng quan về cách chúng ta lưu trữ dữ liệu trong RDBMS, cũng như cách để truy vấn và tham chiếu những dữ liệu này qua nhiều bảng khác nhau với SQL và join. Join là một kỹ thuật rất linh hoạt, tuy nhiên khi số lượng bảng cũng như số record mỗi bảng tăng lên, kỹ thuật này trở nên vô cùng tốn kém về tài nguyên tính toán, vì khi đó sẽ có nhiều nơi hơn cần phải tham chiếu tới, cũng như nhiều tính toán so sánh hơn cần được thực hiện. Có 2 giải pháp cho vấn đề này: sử dụng **Index**, hoặc sử dụng một hệ thống lưu trữ tối ưu hơn. Mình sẽ trình bày giải pháp thứ 2 trong bài viết tiếp theo !
