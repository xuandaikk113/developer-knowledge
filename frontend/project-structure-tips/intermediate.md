# React Intermediate project structure
## assets
Nơi chứa các file như svg, ảnh, ...
## components
Nơi chứa các components dùng chung cho nhiều pages, có thể bao gồm thư mục ui, thư mục form, ...
## context
Các file xử lý context
## data
Các file dữ liệu, hằng, config, ...
## hooks
Các file hooks dùng chung cho nhiều pages
## pages
Các thư mục như Home, Login, Register, Dashboard, Settings, ... trong mỗi thư mục sẽ bao gồm giao diện chính và các component mà chỉ sử dụng cho riêng page đó, hook dùng riêng cho page
## utils
Các hàm xử lý dữ liệu, hàm bổ trợ
## __test__ 
Đặc biệt là các thư mục __test__ phải nằm ngay bên cạnh các file code cần test ví dụ như components chẳng hạn