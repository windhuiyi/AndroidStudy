OkHttp3 使用总结
===============



### OkHttp 设置 header,添加拦截器Interceptor，在拦截器里面添加。

```java
    Headers headers = request.headers().newBuilder().add("token", token).build();
    response = chain.proceed(request.newBuilder().headers(headers).build());
```



### OkHttp3 java.lang.IllegalStateException: closed 错误

- ResponseBody调用string后，会关闭流，再调用这个方法就会报错。

```java
  public final String string() throws IOException {
    BufferedSource source = source();
    try {
      Charset charset = Util.bomAwareCharset(source, charset());
      return source.readString(charset);
    } finally {
      Util.closeQuietly(source);
    }
  }
```