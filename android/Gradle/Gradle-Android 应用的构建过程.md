Gradle-Android 应用的构建过程
======================

### Android 应用的构建过程

![image_build_application](https://github.com/windhuiyi/AndroidStudy/blob/master/images/image_build_application.png?raw=true)

1. 主要的资源文件（layout, values 等）都被 aapt 编译，并且在一个 R 文件中引用
2. Java 代码被 Java 编译器编译成 JVM 字节码(.class 文件)
3. JVM 字节码再被 dex 工具转换成 dalvik 字节码(.dex 文件)
4. 然后这些 .dex 文件、编译过的资源文件和其他资源文件（比如图片）会被打包成一个 apk
5. apk 文件在安装前会被 debug/release 的 key 文件签名
6. 安装到设备

- 几个注意点
  - 上面的步骤中第一步注意是主要的资源文件，有些特别的资源文件就不会被编译，比如 assets 目录下的文件，raw 目录下的文件还有图片，都不会被编译。只不过 raw 下的文件会在 R 文件里生成 id
  - 如果对 apk 正式签名，还需要使用 zipalign 工具对 apk 进行对齐操作，这样做的好处是当应用运行时会减少内存的开销

> 参考文章
> [Gradle for Android 系列：为什么 Gradle 这么火](https://cloud.tencent.com/developer/article/1014645)
