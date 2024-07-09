# Custom hooks
- Custom hook giúp seperate code logic ra khỏi component giúp code dễ maintain và đẹp hơn.
- Một ví dụ sử dụng custom hook vô cùng thực tiễn mà chắc chắn mọi người ai cũng phải dùng đó là fetch data bằng custom hook.
- Tên custom hook phải được bắt đầu bằng use, nếu không sẽ bị lỗi

## Tips for Building Custom Hooks
 
- Start by identifying repetitive logic across your components.
- Extract this logic into a function named with use prefix. Custom hooks should start with use, like useFormInput or useFetch.
- Use React's built-in Hooks within your custom Hook as needed.
- Return anything that will be useful for the component using this Hook.
- Each custom hook should be responsible for a single piece of functionality.

## Ví dụ

### Ví dụ 1: Our First Custom Hook
useCounter.js
```jsx
import { useState } from 'react'

// Custom hook for counter functionality
function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue) // State to keep track of count

  // Function to increment count
  const increment = () => setCount(prevState => prevState + 1)

  // Returns count and increment function
  return [count, increment]
}
```
CounterComponent.jsx
```jsx
import React from 'react';
import useCounter from './useCounter'

// Component using the useCounter Hook
function CounterComponent() {
  const [count, increment] = useCounter() // Utilizing useCounter

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  )
}
```

### Ví dụ 2: Building a Data Fetching Hook
> Sử dụng fetch
useFetch.js
```jsx
import { useState, useEffect } from 'react'

// Custom hook for fetching data
function useFetch(url) {

  const [data, setData] = useState(null) // State for data
  const [loading, setLoading] = useState(true) // State for loading
  const [error, setError] = useState(null) // State for error handling

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true)
      setError(null)

      try {
        const response = await fetch(url)

        if (!response.ok) {
          throw new Error(`An error occurred: ${response.statusText}`)
        }

        const jsonData = await response.json()
        setData(jsonData)
      } catch (error) {
        setError(error.message)
      } finally {
        setLoading(false)
      }
    }

    fetchData()
  }, [url]) // Dependency array with url

  return { data, loading, error }
}
```
SomeComponent.jsx
```jsx
import React from 'react'
import useFetch from './useFetch'

// Component using the useFetch Hook
function DataDisplayComponent({ url }) {
  const { data, loading, error } = useFetch(url) // Utilizing useFetch

  if (loading) return <p>Loading...</p>
  if (error) return <p>Error: {error}</p>

  return <div>{JSON.stringify(data, null, 2)}</div>
}
```
> Sử dụng Axios

useFetch.js
```jsx
import { useState, useEffect } from 'react';
import axios from 'axios';

const useFetch = (url) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await axios.get(url);
        setData(response.data);
        setError(null);
      } catch (err) {
        setError(err);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
};

export default useFetch;
```
DataFetchingComponent.jsx
```jsx
import React from 'react';
import useFetch from './useFetch';

const DataFetchingComponent = () => {
  const { data, loading, error } = useFetch('https://api.example.com/data');

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <div>
      <h1>Fetched Data:</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default DataFetchingComponent;
```
### Ví dụ 3: Building a Form Handling Hook
hook.js
```jsx
import { useState } from 'react'

function useFormInput(initialValue) {
  const [value, setValue] = useState(initialValue)

  const handleChange = (e) => {
    setValue(e.target.value)
  }

  // Returns an object with value and onChange properties
  return {
    value,
    onChange: handleChange
  }
}
```
component.jsx
```jsx
import useFormInput from './useFormInput'

function FormComponent() {

  const name = useFormInput('')
  const age = useFormInput('')

  const handleSubmit = (e) => {
    e.preventDefault()
    console.log(`Name: ${name.value}, Age: ${age.value}`)
  }

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>
          Name:
          {/* The spread operator here is equivalent to value={name.value} onChange={name.onChange} */}
          <input type="text" {...name} />
        </label>
      </div>
      <div>
        <label>
          Age:
          <input type="number" {...age} />
        </label>
      </div>
      <button type="submit">Submit</button>
    </form>
  )
}
```



