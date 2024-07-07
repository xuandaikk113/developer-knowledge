# useState hook - Trạng thái của dữ liệu

## Dùng khi nào?
Khi muốn dữ liệu thay đổi thì giao diện tự động cập nhật (render lại theo dữ liệu)

Video tham khảo:
https://www.youtube.com/watch?v=CVaEWBFpxhc&list=PL_-VfJajZj0W8-gf0g3YCCyFh-pVFMOgy&index=3



## Cách dùng
``` jsx
import {useState} from 'react'

function Component() {
    const [state, setState] = useState(initState)
    ...
}
```
## Ví dụ
``` jsx
import { useState } from 'react'
function App() {
    const [counter , setCounter] = useState(0)
    const handleClick = () => {
        setCounter(counter + 1)
    }
    return (
        <div>
            <h1>Count: {counter}</h1>
            <button onClick={handleClick}>Increase</button>
        </div>
    )
}
export default App
```

## Lưu ý
- Hook chỉ dùng với function component
- Component được re-render sau khi 'setState'
- Initial state chỉ dùng cho lần đầu
- Set state với callback?
  `Khi sử dụng set state với callback, setState sẽ trả về một đối số là preState, ta có thể dùng đối số này để tuỳ biến như ví dụ sau`
  ``` jsx
  setCounter(preState => preState + 1)
  ```
- Initial state với callback?
  `Nhận vào 1 hàm callback và lấy giá trị return của hàm đó để làm giá trị initial state, hàm callback này chỉ chạy 1 lần duy nhất khi khởi tạo (thích hợp nếu hàm callback tính toán nặng nề)`
- Set state là thay thế state bằng giá trị mới
  `Vì setState là thay thế state bằng giá trị mới nên khi state là 1 object, ta phải xử lý như sau để chương trình hoạt động đúng theo ý muốn`
  ``` jsx
    import { useState } from 'react'
    function App() {
    const [info , setInfo] = useState({
        name: 'Nguyen Van A',
        age: 18,
        address: 'Ha Noi'
    })

    const handleUpdate = () => {
        setInfo({
            ...info,
            bio: "Yeu mau hong"
        })
    }
    return (
        <div>
            <h1>Info: {JSON.stringify(info)}</h1>
            <button onClick={handleUpdate}>Update</button>
        </div>
    )
    }
    export default App
  ```



