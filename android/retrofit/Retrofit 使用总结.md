Retrofit 学习
===========

### Retrofit 动态URL @GET Url地址和baseUrl不同，如何设置？

- 设置`@Url`参数

```java
    @GET
    public Call<ResponseBody> profilePicture(@Url String url);
```


