CheckBox 汇总
===========

### CheckBox 自定义 Button 样式

(1)在drawable文件夹中添加drawable文件checkbox_style.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
 
    <item android:drawable="@drawable/checkbox_check" android:state_checked="true"/>
    <item android:drawable="@drawable/checkbox_normal" android:state_checked="false"/>
 
</selector>
```

(2) 在 CheckBox 设置 `button`

```xml
android:button="@drawable/checkbox_style"
```