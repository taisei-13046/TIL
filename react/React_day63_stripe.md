## やったこと
axiosについて

### interceptors
[Interceptors](https://axios-http.com/docs/interceptors)  
[axiosのinterceptorsで、リクエストの前処理を共通して行う](https://qiita.com/buntafujikawa/items/78e9204cc9ea7eaabd3d)  

```tsx
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


















