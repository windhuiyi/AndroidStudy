Dialog 汇总
=========

### Dialog 自定义大小

```java
        WindowManager wm = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
        Display display = wm.getDefaultDisplay(); //获取屏幕宽高
        Point point = new Point();
        display.getSize(point);
        // 方式一
        Window window = getWindow();
        WindowManager.LayoutParams layoutParams = window.getAttributes(); //获取当前对话框的参数值
        layoutParams.width = (int) (point.x * 0.5); //宽度设置为屏幕宽度的0.5
        layoutParams.height = (int) (point.y * 0.5); //高度设置为屏幕高度的0.5
        window.setAttributes(layoutParams);
        // 方式二 
        window.setLayout((int) (point.x * 0.5), (int) (point.y * 0.5));
```
Window `setLayout` 方法

```java
    public void setLayout(int width, int height) {
        final WindowManager.LayoutParams attrs = getAttributes();
        attrs.width = width;
        attrs.height = height;
        dispatchWindowAttributesChanged(attrs);
    }

```
### Dialog 透明 半透明

1) XML 设置

- 透明
```xml
<style name="Translucent_NoTitle" parent="android:style/Theme.Dialog">
    <item name="android:background">#00000000</item> <!-- 设置自定义布局的背景透明 -->
    <item name="android:windowBackground">@android:color/transparent</item>  <!-- 设置window背景透明，也就是去边框 -->
</style>
```

- 半透明
```xml
  <style name="Transparent  ">  
    <item name="android:windowBackground">@color/transparent_background</item>  
    <item name="android:windowNoTitle">true</item>  
    <item name="android:windowIsTranslucent">true</item>    
    <item name="android:windowAnimationStyle">@+android:style/Animation.Translucent</item>  
  </style>  
```

2）代码设置

```java
    // 
    WindowManager.LayoutParams lp=window.getAttributes();
    lp.dimAmount=0.2f;  // 遮罩层透明度，0.0f~1.0f， 1.0f为完全不透明,dialog周围都是黑的
    window.setAttributes(lp);
```
