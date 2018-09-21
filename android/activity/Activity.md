Activity 使用相关知识点
================

### Activity 全局竖屏或横屏

- 通常在`AndroidManifest`里面的`activity`标签设置`screenOrientation`为`portrait`或`landscape`。

```xml
        <activity
            android:name=".ui.activity.MainActivity"
            android:exported="false"
            android:launchMode="singleTask"
            android:screenOrientation="portrait"
            android:theme="@style/AppTheme" />
```

- 但是这样只是针对一个 Activity。如果要多个 Activity 都是竖屏或横屏，可以在 BaseActivity的`onCreate`中代码设置。

```java
    // 设置为竖屏
    setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);
    // 设置为横屏
    setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);    
```

### Activity 的 launchMode 解析

#### 如果有 A、B、C、D 四个 Activity,启动顺序为 A > B > C > D > B.



