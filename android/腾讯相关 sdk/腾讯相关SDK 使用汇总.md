腾讯相关SDK 使用汇总
===========

### 微信分享报错 ERR_BAN
- 签名错误导致。使用的是32位小写的MD值，可以使用Gradle的Tasks 下面的android的signingReport命令来查看。

```java
Variant: release
Config: release
Store: /../../../../test.keystore
Alias: com.test
MD5: 24:53:3F:02:68:1C:2B:33:0C:78:8A:82:03:6A:33:E8
SHA1: 55:37:A7:88:21:A3:44:F7:E8:D6:9C:4B:51:65:1C:27:BD:55:5F:38
Valid until: 2044年1月9日 星期六
```

### 微信支付 提示缺少packageValue
```java 
request.packageValue = "Sign=WXPay"; // 接口没有返回。自己加上。
```

### 微信登录 SendAuth.Req 设置
```java
    SendAuth.Req req = new SendAuth.Req();
    req.scope = "snsapi_userinfo"; // 应用授权作用域，如获取用户个人信息则填写snsapi_userinfo
    req.state = "wx_login"; //用于保持请求和回调的状态，授权请求后原样带回给第三方。该参数可用于防止csrf攻击（跨站请求伪造攻击），建议第三方带上该参数，可设置为简单的随机数加session进行校验
```