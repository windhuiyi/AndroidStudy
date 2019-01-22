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



### Retrofit BaseUrl 报错问题

```java
Retrofit.Builder()
                .addConverterFactory(GsonConverterFactory.create())
                .addCallAdapterFactory(new LiveDataCallAdapterFactory())
                .baseUrl("http://api.example.com/api/") // 最后必须带个/ 不然会报错
                .client(okHttpClient)
                .build()
                .create(Test.class);
```

- 如果你在注解中提供的url是完整的url，则url将作为请求的url。
```java
@GET("http://api.example.com/api/member/address/exitaddress")
实际请求地址http://api.example.com/api/member/address/exitaddress
```
- 如果你在注解中提供的url是不完整的url，且不以 / 开头，则请求的url为baseUrl+注解中提供的值
```java
@GET("member/address/exitaddress")
实际请求地址http://api.example.com/api/member/address/exitaddress
```
- 如果你在注解中提供的url是不完整的url，且以 / 开头，则请求的url为baseUrl的主机部分+注解中提供的值
```java
@GET("/member/address/exitaddress")
实际请求地址http://api.example.com/member/address/exitaddress 会少了/api
```