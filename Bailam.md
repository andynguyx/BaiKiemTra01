Câu 1: 
- Value Types (kiểu giá trị) là kiểu dữ liệu mà biến lưu trực tiếp giá trị của dữ liệu. Khi gán một biến cho biến khác, giá trị được sao chép nên hai biến hoạt động độc lập. Các kiểu thường gặp gồm int, double, bool, struct, enum.
- Reference Types (kiểu tham chiếu) là kiểu dữ liệu mà biến lưu tham chiếu đến một đối tượng trong vùng nhớ Heap. Khi gán một biến cho biến khác, tham chiếu được sao chép nên hai biến có thể cùng trỏ đến một đối tượng.
- Về bộ nhớ, cách hiểu cơ bản là Value Type thường được lưu trên Stack, còn đối tượng của Reference Type được lưu trên Heap. Tuy nhiên, đây là cách mô tả đơn giản; vị trí thực tế còn phụ thuộc vào ngữ cảnh thực thi của .NET.

Câu 2: 
-Set cho phép thuộc tính được gán và thay đổi giá trị nhiều lần trong suốt vòng đời của đối tượng.
-init là tính năng từ C# 9, cho phép thuộc tính được thiết lập trong quá trình khởi tạo đối tượng, nhưng sau khi đối tượng được khởi tạo thì không thể thay đổi giá trị thông qua thuộc tính đó.
-Trường hợp sử dụng: init phù hợp với các thuộc tính chỉ cần thiết lập một lần, chẳng hạn mã sinh viên, mã sản phẩm, ngày tạo đối tượng. Điều này giúp hạn chế việc dữ liệu bị thay đổi ngoài ý muốn.

Câu 3: 
- Virtual là từ khóa được sử dụng ở lớp cha để khai báo một phương thức có thể được lớp con ghi đè.
-Override được sử dụng ở lớp con để cung cấp cách triển khai mới cho phương thức virtual của lớp cha.
- Sự kết hợp giữa virtual và override cho phép C# thực hiện tính đa hình (Polymorphism). Khi một đối tượng thuộc lớp con được tham chiếu bằng kiểu của lớp cha, chương trình có thể thực thi phiên bản phương thức được ghi đè của lớp con.

Câu 4: 
- Thành phần static thuộc về lớp (Class) chứ không thuộc về một đối tượng cụ thể được tạo bằng new.
- Một thành viên static chỉ tồn tại một bản dùng chung cho toàn bộ lớp, trong khi mỗi Object Instance có các thành viên riêng của nó.
- Vì vậy, thành phần static được truy xuất thông qua tên lớp, thay vì thông qua Object Instance.
- Ví dụ: Nếu Count là thành viên static của lớp Student, cách truy cập đúng là Student.Count.
