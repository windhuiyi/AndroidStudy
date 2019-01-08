Animation 使用总结
=================

### 图片均速旋转

- 在 res 文件夹下建立 anim 文件夹。
- 添加关于 animation 的xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" >
    <!-- 时间 -->
    <!-- 开始角度 -->
    <!-- 起始 x 轴位置 -->
    <!-- 起始 y 轴位置 -->
    <!-- 重复次数，-1循环 -->
    <!-- 重复模式 -->
    <!-- 结束角度 -->
    <rotate
        android:duration="500"
        android:fromDegrees="0"
        android:pivotX="50%"
        android:pivotY="50%"
        android:repeatCount="-1"
        android:repeatMode="restart"
        android:toDegrees="360" />
</set>

```

- 加载动画、设置Interpolator、然后启动动画。
```java
Animation animation = AnimationUtils.loadAnimation(this,R.anim.img_animation);
LinearInterpolator lin = new LinearInterpolator();//设置动画匀速运动
animation.setInterpolator(lin);
imageView.startAnimation(animation);
```