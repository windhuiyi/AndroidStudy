Glide 使用 分析
============

### Glide 版本升级

- 新建一个类，添加注解`@GlideModule`和继承`AppGlideModule`

```java
@GlideModule
public final class BaseAppGlideModule extends AppGlideModule {}
```

- 之前的Glide用法有些可能还适用，有些可能要改为GlideApp

```java
// 普通用法还能继续适用
 Glide.with(imageView.getContext()).load(url).into(imageView);
// 要用到占位图之类的，主要用GlideApp
GlideApp.with(imageView.getContext()).load(url).placeholder(load).error(error).into(imageView);
```



- 新版Glide 圆形图片、圆角图片

```java
        // 单个变换 圆形CircleCrop
        GlideApp.with(imageView.getContext()).load(url).transform(new CircleCrop()).into(imageView);
        // 多个变换 CenterCrop 圆角RoundedCorners
        GlideApp.with(imageView.getContext()).load(url).transforms(new CenterCrop(),new RoundedCorners(10)).into(imageView);
        // 使用apply 单个
        GlideApp.with(imageView.getContext()).load(url).apply(new RequestOptions().transform(new RoundedCorners(10))).into(imageView);
        // 使用apply 多个方式1
        GlideApp.with(imageView.getContext()).load(url).apply(new RequestOptions().transform(new MultiTransformation<>(new CenterCrop(),new RoundedCorners(10)) )).into(imageView);
         // 使用apply 多个方式2
GlideApp.with(imageView.getContext()).load(url)..apply(new RequestOptions().transforms(new CenterCrop(),new RoundedCorners(10))).into(imageView);

```