MediaPlayer 使用总结
================
### BUG : mediaplayer Failed to reset optimization [3, 0]

#### 原因： MediaPlayer 使用 `reset` 之后，没有接着使用`setDataSource` 和 `prepare` 方法。

```java
//Resets the MediaPlayer to its uninitialized state. After calling this method, 
//you will have to initialize it again by setting the data source and calling prepare().
      mMediaPlayer.reset();
      uri = Uri.parse("android.resource://" + context.getPackageName() + "/" + R.raw.wait_for_answer);
      mMediaPlayer.setDataSource(context, uri);
      mMediaPlayer.prepare();
```
