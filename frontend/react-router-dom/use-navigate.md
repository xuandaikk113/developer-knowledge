# useNavigate
Hook dùng để navigate đến các route đã khai báo của react-router-dom (trong App.jsx)

Ví dụ:

App.jsx có các route như "/", "/books/create", "/books/details/:id", ...
```jsx
import React from "react";
import { Routes, Route } from "react-router-dom";
import Home from './pages/Home'
import CreateBook from './pages/CreateBook'
import ShowBook from './pages/ShowBook'
import EditBook from './pages/EditBook'
import DeleteBook from './pages/DeleteBook'

const App = () => {
  return (
    <Routes>
      <Route path='/' element={<Home/>} />
      <Route path='/books/create' element={<CreateBook/>} />
      <Route path='/books/details/:id' element={<ShowBook/>} />
      <Route path='/books/edit/:id' element={<EditBook/>} />
      <Route path='/books/delete/:id' element={<DeleteBook/>} />
    </Routes>
  )
};

export default App;
```
Import và khai báo
```jsx
import { useNavigate } from "react-router-dom";

const EditBook = () => {
    const navigate = useNavigate();
    const handleEditBook = () => {
        const data = {
        title,
        author,
        publishYear,
        };
        setLoading(true);
        axios
        .put(`http://localhost:5555/books/${id}`, data)
        .then(() => {
            setLoading(false);
            enqueueSnackbar("Book edited successfully!", { variant: "success" });
            navigate("/");
        })
        .catch((error) => {
            setLoading(false);
            enqueueSnackbar("Book edited failed!", { variant: "error" });
            console.log(error);
        });
    };
}

```
Có thể thấy, ở đoạn code bên trên, sau khi gọi api hoàn tất, navigate về trang chủ "/" (về element Home)