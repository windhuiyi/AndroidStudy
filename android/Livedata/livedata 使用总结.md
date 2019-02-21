livedata 使用总结
=============

### How can I perform LiveData transformations on a background thread?

[How can I perform LiveData transformations on a background thread?](https://stackoverflow.com/questions/47374580/how-can-i-perform-livedata-transformations-on-a-background-thread)



### LiveData observeForever 永久观察， 不理会生命周期

- 主要AlwaysActiveObserver将observer进行封装。

```java
 AlwaysActiveObserver wrapper = new AlwaysActiveObserver(observer);
```

- AlwaysActiveObserver的shouldBeActive总是返回 true
```java
        @Override
        boolean shouldBeActive() {
            return true; // 总是返回 true
        }
```