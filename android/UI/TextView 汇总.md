TextView 汇总
===========

### TextView 在`text`属性里面设置换行符 `\n` 换行不成功。

- 需要在`strings.xml`里面设置`string`就行了。

###  Textview 不依赖 ScrollView 滑动

- 在 xml 设置

```
android:scrollbars="vertical"  
```

- 在代码设置

```
tv.setMovementMethod(ScrollingMovementMethod.getInstance());
```
