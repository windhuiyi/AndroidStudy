阿里相关SDK汇总
==============

### OSS 上传报错 

- 使用OSS SDK上传图片报错
```java
com.alibaba.sdk.android.oss.ClientException: read failed: EBADF (Bad file descriptor) [ErrorMessage]: read failed: EBADF (Bad file descriptor)
```
- 解决办法：尼玛，是因为Android Profiler分析Network，为了分析OKHttp的网络请求加拦截器的导致的，取消了Profiling 的勾选就选就可以。

  ![profiling](/Users/windhuiyi/code/AndroidStudy/images/profiling.png)

