OkHttp3 使用总结
===============



### OkHttp 设置 header,添加拦截器Interceptor，在拦截器里面添加。

```java
    Headers headers = request.headers().newBuilder().add("token", token).build();
    response = chain.proceed(request.newBuilder().headers(headers).build());
```



