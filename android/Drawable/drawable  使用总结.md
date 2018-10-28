drawable 使用总结
=============

### 获取 `drawable` 路径

```java
private String getResourcesUri(@DrawableRes int id) {  
        Resources resources = getResources();  
        String uriPath = ContentResolver.SCHEME_ANDROID_RESOURCE + "://" +  
                resources.getResourcePackageName(id) + "/" +  
                resources.getResourceTypeName(id) + "/" +  
                resources.getResourceEntryName(id);  
        return uriPath;  
    }
```

### `selector` 中定义 shape, 少建一个 xml 文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_window_focused="false">
        <shape android:shape="rectangle">
            <solid android:color="@color/white" />
            <stroke android:width="1dp" android:color="@color/blue" android:dashGap="2dp" android:dashWidth="10dp" />
            <corners android:radius="2dp" />
        </shape>
    </item>
    <item android:state_pressed="true">
        <shape android:shape="rectangle">
            <solid android:color="@color/white" />
            <stroke android:width="1dp" android:color="@color/green" android:dashGap="2dp" android:dashWidth="10dp" />
            <corners android:radius="2dp" />
        </shape>
    </item>
</selector>

```



###   gradient  颜色渐变 

[Android View — Gradient 渐变]: https://www.jianshu.com/p/32d17739d378

```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
	<!-- 起始角度，0 左边；90 下边，180，右边，270 上边。共8个方向，从0开始每次增加45 -->
    <gradient
        android:angle="0"
        android:endColor="#FF3CC929"
        android:centerColor="#FF00C23F"
        android:startColor="#FF01AF66" />
</shape>
```

