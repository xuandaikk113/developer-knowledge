# So sánh Interface và Abstract Class trong OOP
## Ưu điểm:
### Interface
Có thể kế thừa nhiều interface(tính đa hình).
Xây dựng được bộ khung mẫu mà các lớp phải follow theo.
Giúp quản lý tốt, nắm bắt được các chức năng phải có cho một đối tượng nào đó.
### Abstract class
Có thể linh động các method. giống như một class thông thường.
Các class extend có thể override hoặc không override các method thường.
## Nhược điểm:
### Interface:
Mỗi khi định nghĩa thêm tính năng, các class impelement nó đồng lọat phải thêm tính năng đó, khả năng cao sẽ không có xử lý gì.
### Abstract class
Không thể extend nhiều abstract class.

# Khi nào sử dụng chúng:
## Interface: 
Khi bạn muốn tạo dựng một bộ khung chuẩn gồm các chức năng mà những module hay project cần phải có. Giống như sau khi nhận requirement của khách hàng về team ngồi với nhau và phân tích các đầu mục các tính năng của từng module, sau đó triển khai vào code viết các interface như đã phân tích,để các bạn dev có thể nhìn vào đó để thực hiện đủ các tính năng (khi đã implement rồi thì không sót một tính năng nào ^^).
## Abstract class: 
Giống như demo trên bạn có thể hiểu khi định nghĩa một đối tượng có những chức năng A,B,C trong đó tính năng A,B chắc chắn sẽ thực thi theo cách nào đó, còn tính năng C phải tùy thuộc vào đối tượng cụ thể là gì, như đối tượng Dog, Cat tuy chúng đều có thể phát ra âm thanh nhưng âm thanh là khác nhau. Vì vậy method Speak() là abstract method để chỉ ra rằng tính năng này còn dang dở chưa rõ thực thi, các lớp extend phải hoàn thành nốt tính năng này, còn những tính năng đã hoàn thành vẫn sử dụng như bình thường đây là những tính năng chung.