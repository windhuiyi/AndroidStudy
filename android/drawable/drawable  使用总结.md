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



