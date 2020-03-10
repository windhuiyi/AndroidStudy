Android 适配
===========

### 小米 Mix3 全面屏 下边黑边，没有全屏

- 在 AndroidManifest.xml 的 <application> 标签里加入
```xml
 <meta-data android:name="android.max_aspect" android:value="2.2" />
```
>参考
>[Android适配全面屏上下黑边问题](https://www.jianshu.com/p/db78a0272727)
>[适配全面屏手机](http://blog.huangyuanlove.com/2018/11/12/%E9%80%82%E9%85%8D%E5%85%A8%E9%9D%A2%E5%B1%8F%E6%89%8B%E6%9C%BA/)
>[Android APP适配全面屏手机的技术要点](https://www.jianshu.com/p/e164dec92bd8)
>[今日头条屏幕适配方案终极版正式发布!](https://www.jianshu.com/p/4aa23d69d481)
>
>
