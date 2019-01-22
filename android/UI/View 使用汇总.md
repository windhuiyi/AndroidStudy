

View 使用汇总
============

### View 代码设置 padding 和 margin
```java
// padding 是视图本身的属性，直接设置就可以。
ImageView imageView = new ImageView(this);
imageView.setPadding(5,5,5,5)
// margin 是视图和其他视图的交互，需要设置LayoutParams，根据它外面那层的视图的LayoutParams设置
LinearLayout layout = (LinearLayout) convertView.findViewById(R.id.linearlayout);
ImageView imageView = new ImageView(this);
LinearLayout.LayoutParams params = new LinearLayout.LayoutParams(100, 100);
params.setMargins(5, 5, 5, 5);
layout.addView(imageView);
```



