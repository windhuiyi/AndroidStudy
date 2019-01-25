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