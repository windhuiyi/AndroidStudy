Retrofit 学习
===========

### Retrofit 动态URL @GET Url地址和baseUrl不同，如何设置？

- 设置`@Url`参数

```java
    @GET
    public Call<ResponseBody> profilePicture(@Url String url);
```

### Retrofit 设置 timeout

-  通过`OkHttpClient`设置 timeout 时间，有连接、读、写三种 timeout。

```java
        OkHttpClient.Builder builder = new OkHttpClient.Builder();
        builder.connectTimeout(1000,TimeUnit.MILLISECONDS);
        builder.readTimeout(1000,TimeUnit.MILLISECONDS);
        builder.writeTimeout(1000,TimeUnit.MILLISECONDS);
        
```



