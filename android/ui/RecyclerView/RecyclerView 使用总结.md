RecyclerView 使用总结
=================

### RecyclerView 滑动到底部

- 使用`LinearLayoutManager`的`scrollToPositionWithOffset`方法

> 这个方法会滚动到指定的位置, 并且是置顶显示. 第二个参数可以决定 距离顶部的offset 偏移量;
  如果你传了一个不存在的position, 那么这个方法啥也不干.
  并且并不会加载所有滚动经过的View, 只会加载 position 当前页能显示的View;

```java
    public void scrollToPositionWithOffset (int position, int offset)
```

###### 参考文章

- [Android-->RecyclerView 显示底部,滚动底部(无动画)](https://blog.csdn.net/angcyo/article/details/53066925)