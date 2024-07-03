# useEffect hook
Chuyên dùng để xử lý side effects (Hoạt động ngoài lề so với ứng dụng chính)

## Dùng khi nào?
- Update DOM
- Call API
- Listen DOM events (scroll, resize, ...)
- Cleanup (remove listeners / unsubscribe, clear timer) 

## Cần nhớ
### Cú pháp
``` jsx
useEffect(callback, [dependencies])
```
Callback là bắt buộc, dependencies không bắt buộc
### Các trường hợp sử dụng
- TH1. useEffect(callback) - rất ít dùng trong thực tế
  - Gọi callback mỗi khi component re-render
  - Gọi callback sau khi component thêm element vào DOM
- TH2. useEffect(callback, []) - thường áp dụng cho việc gọi API, những logic chỉ muốn chạy 1 lần
  - Chỉ gọi callback 1 lần sau khi component mounted
- TH3. useEffect(callback, [dependencies])
  - Callback sẽ được gọi lại mỗi khi dependencies thay đổi

`Lưu ý chung:`
1. Callback luôn được gọi sau khi component mounted

## Cú pháp
Là một hàm nhận 2 đối số
``` jsx
function Content() {

}
```