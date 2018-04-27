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



