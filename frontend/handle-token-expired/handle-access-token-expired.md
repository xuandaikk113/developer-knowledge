# Using Interceptors
You can intercept requests or responses before they are handled by then or catch.
``` javascript
// Add a request interceptor
axios.interceptors.request.use(function (config) {
    // Do something before request is sent
    return config;
  }, function (error) {
    // Do something with request error
    return Promise.reject(error);
  });

// Add a response interceptor
axios.interceptors.response.use(function (response) {
    // Any status code that lie within the range of 2xx cause this function to trigger
    // Do something with response data
    return response;
  }, function (error) {
    // Any status codes that falls outside the range of 2xx cause this function to trigger
    // Do something with response error
    return Promise.reject(error);
  });
```
If you need to remove an interceptor later you can.
``` javascript
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```
You can add interceptors to a custom instance of axios.
``` javascript
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```
---
# Another reference
Recently I was asked to create an integration with the use of an API. This API used OAuth for authentication. After a successful login, the API token and refresh token are returned. You know, as you would expect these days.

After the token expires, you'll need to request a new token using the refresh token. Then, you need to use the freshly retrieved token for the next API requests until, of course that token will expire as well.

You could send that request for a new token after the first 403 response. But with the Axios library you can also use interceptors. An example using such an interceptor can be found below.

I'm using Redis to store the token and refresh token received from the API as you can see in the examples. Obvious code is omitted and simplified where applicable.
``` javascript
const axios = require('axios');
const axiosApiInstance = axios.create();
// Request interceptor for API calls
axiosApiInstance.interceptors.request.use(
  async config => {
    const value = await redisClient.get(rediskey)
    const keys = JSON.parse(value)
    config.headers = { 
      'Authorization': `Bearer ${keys.access_token}`,
      'Accept': 'application/json',
      'Content-Type': 'application/x-www-form-urlencoded'
    }
    return config;
  },
  error => {
    Promise.reject(error)
});
// Response interceptor for API calls
axiosApiInstance.interceptors.response.use((response) => {
  return response
}, async function (error) {
  const originalRequest = error.config;
  if (error.response.status === 403 && !originalRequest._retry) {
    originalRequest._retry = true;
    const access_token = await refreshAccessToken();            
    axios.defaults.headers.common['Authorization'] = 'Bearer ' + access_token;
    return axiosApiInstance(originalRequest);
  }
  return Promise.reject(error);
});
```
The response interceptor checks to see if the API returned a 403 status due to an expired token. If so, it calls a function to refresh the access token which it uses for its call. That function (refreshAccessToken) is an Axios call to the auth service on the API which returns and stores the token and refreshtoken in Redis.

Now using these interceptors you can call the API like;
``` javascript
const result = await axiosApiInstance.post(url, data)
```