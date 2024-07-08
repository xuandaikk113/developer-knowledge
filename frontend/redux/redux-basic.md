# Redux
Redux là thư việc giúp quản lý các state, các variables trong react một cách độc lập mà không cần phải truyền từ component cha sang component con

## Video tham khảo
[https://www.youtube.com/watch?v=iBUJVy8phqw]

## Bước 1: Tạo redux store và bao bọc ở top level component của app để các component con bên dưới có thể access bất cứ khi nào (một store có thể có nhiều reducers)

## Bước 2: Tạo reducer với biến và các method thay đổi biến

## Bước 3: Gọi biến từ reducer ra để hiển thị bằng useSelector() hoặc gọi các method của reducer để thay đổi biến bằng useDispatch()