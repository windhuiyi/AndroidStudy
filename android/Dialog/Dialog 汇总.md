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
