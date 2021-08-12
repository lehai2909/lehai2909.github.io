# Cách chúng ta lưu trữ dữ liệu đã thay đổi như thế nào?

## Từ Hệ quản trị cơ sở dữ liệu đến Data Lake

Có rất nhiều thứ đã diễn ra trong hàng chục năm qua: sự bùng nổ của dữ liệu lớn, sự ra đời của các nền tảng tính toán phân tán như Hadoop, Spark để đáp ứng (dù chỉ là một phần) nhu cầu tính toán trên dữ liệu lớn, với mục đích cuối cùng theo mình được tóm tắt trong một biểu thức đơn giản: **Data => Information**

Bên cạnh khả năng tính toán, một thứ khác luôn luôn song hành và phải được nhắc tới: khả năng lưu trữ. Qua từng giai đoạn, lại có những mô hình lưu trữ mới được tạo ra, nhằm bù đắp những thiếu sót của các mô hình cũ, hay để đáp ứng yêu cầu lưu trữ ngày càng nhanh và đa dạng của dữ liệu lớn. 2 ví dụ điển hình mà chúng ta có thể nêu ra:

- Sự chuyển đổi từ việc sử dụng các spreadsheet (bảng tính) như Excel đến sự ra đời và bùng nổ của các hệ cơ sở dữ liệu quan hệ (RDBMS) như Oracle Database hay MySQL, theo sau bằng sự ra đời của các hệ cơ sở dữ liệu NoSQL
- Sự xuất hiện của khái niệm Data Lake dần dần thay thế mô hình Data Warehouse truyền thống

Nếu bạn cảm thấy xa lạ và hốt hoảng với những khái niệm này, đừng lo ::smile::

