TabLayout 使用汇总
================

### TabLayout 添加Tab
```java
tabLayout.addTab(fullSearch_tabLayout.newTab());
```

### TabLayout 选中背景

- 添加 selector,命名为sl_tab.xml
```Xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="false">
    <shape xmlns:android="http://schemas.android.com/apk/res/android" >
    <solid android:color="#09000000" />
    </shape>
    </item>
    <item android:state_selected="true" >
    <shape xmlns:android="http://schemas.android.com/apk/res/android" >
    <solid android:color="#3F51B5" />
    </shape>
    </item>
</selector>

```

- 设置TabLayout的属性`tabBackground`
```xml
 app:tabBackground="@drawable/sl_tab"
```


### TabLayout的 默认选中
```java
tabLayout.getTabAt(postision).select(); 
```

### TabLayout 点击有阴影

- 28.0.0以前,在xml进行以下设置。
```java
app:tabBackground="@android:color/transparent"
```

- 28.0.0,在 xml 设置，或者代码设置
```java
app:tabRippleColor="@android:color/transparent"
tabLayout.setTabRippleColor(null);
```

- 具体代码在TabView的updateBackgroundDrawable的方法
```java
  private void updateBackgroundDrawable(Context context) {
            if (TabLayout.this.tabBackgroundResId != 0) {
                this.baseBackgroundDrawable = AppCompatResources.getDrawable(context, TabLayout.this.tabBackgroundResId);
                if (this.baseBackgroundDrawable != null && this.baseBackgroundDrawable.isStateful()) {
                    this.baseBackgroundDrawable.setState(this.getDrawableState());
                }
            } else {
                this.baseBackgroundDrawable = null;
            }

            Drawable contentDrawable = new GradientDrawable();
            ((GradientDrawable)contentDrawable).setColor(0);
            Object background;
            // tabRippleColorStateList 是否为空
            if (TabLayout.this.tabRippleColorStateList != null) {
                GradientDrawable maskDrawable = new GradientDrawable();
                maskDrawable.setCornerRadius(1.0E-5F);
                maskDrawable.setColor(-1);
                ColorStateList rippleColor = RippleUtils.convertToRippleDrawableColor(TabLayout.this.tabRippleColorStateList);
                if (VERSION.SDK_INT >= 21) {
                    background = new RippleDrawable(rippleColor, TabLayout.this.unboundedRipple ? null : contentDrawable, TabLayout.this.unboundedRipple ? null : maskDrawable);
                } else {
                    Drawable rippleDrawable = DrawableCompat.wrap(maskDrawable);
                    DrawableCompat.setTintList(rippleDrawable, rippleColor);
                    background = new LayerDrawable(new Drawable[]{contentDrawable, rippleDrawable});
                }
            } else {
                background = contentDrawable; // tabRippleColorStateList 为空
            }

            ViewCompat.setBackground(this, (Drawable)background);
            TabLayout.this.invalidate();
        }
```

