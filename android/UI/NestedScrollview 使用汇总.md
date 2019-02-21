NestedScrollview 使用汇总
======================

### NestedScrollview 嵌套 RecyclerView 滑动卡顿
- 代码设置
```java
recyclerView.setNestedScrollingEnabled(false); // 设置滑动为 false
```
- xml 设置
```xml
android:nestedScrollingEnabled="false" // 要21版本以上才行。
```