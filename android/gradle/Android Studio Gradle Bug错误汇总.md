Android Studio Gradle Bug错误汇总
=============================
####  Run 的时候报错，`apk does not exist on disk.`

- 解决办法：尼玛，也不知道为什么。。。点一下`Gradle projects`的那个刷新图标就行了。

![android_studio_gradle_01](/images/android_studio_gradle_01.png "android_studio_gradle_01")

#### Error:All flavors must now belong to a named flavor dimension.

- 解决办法：Learn more at https://d.android.com/r/tools/flavorDimensions-missing-error-message.html ，就在这个网址可以找到答案。
    - 要解决此错误，请将每个风味分配至一个给定维度，如下面的示例中所示。 由于依赖项匹配现在由插件处理，您应当谨慎地为风味维度命名。例如，如果您的所有应用和库模块均使用 foo 维度，您对插件可以匹配的风味的控制力较弱。

```java
// Specifies a flavor dimension.
flavorDimensions "color" // 就是定义一个这个

productFlavors {
     red {
      // Assigns this product flavor to the 'color' flavor dimension.
      // This step is optional if you are using only one dimension.
      dimension "color" // 然后每个里面加上这个
      ...
    }

    blue {
      dimension "color"
      ...
    }
}

```
