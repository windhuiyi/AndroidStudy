Android Studio Gradle Bug错误汇总
=============================

###  Run 的时候报错，`apk does not exist on disk.`

- 解决办法：点一下`Gradle projects`的那个刷新图标就行了。

![android_studio_gradle_01](/images/android_studio_gradle_01.png "android_studio_gradle_01")

### Error:All flavors must now belong to a named flavor dimension.

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

###  `gradlew: command not found`

- 解决办法：不要运行`gradlew`,运行`./gradlew`,不过一开始还是不行的。。。需要先运行`chmod ./gradlew`

### Android Studio 自带模拟器没有Wi-Fi

- Wifi is not available on the emulator if you are using below of API level 25.所以要搞个25或以上的虚拟机。

### Android Studio 模拟器 网络类型和速度

|  # |  Network Type | Full Name                                  | Speed                                  |
|:-- |:--:           |:--:                                        |:---:                                   |
|  1 | GSM           | Global System for Mobile Communications    | (up: 1.8 KiB/s, down: 1.8 KiB/s)       |
|  2 | HSCSD         | High-Speed Circuit-Switched Data           | (up: 1.8 KiB/s, down: 7.0 KiB/s)       |
|  3 | GPRS          | Generic Packet Radio Service               | (up: 3.5 KiB/s, down: 7.0 KiB/s)       |
|  4 | UMTS          | Universal Mobile Telecommunications System | (up: 46.9 KiB/s, down: 46.9 KiB/s)     |
|  5 | EDGE          | Enhanced Data rates for GSM Evolution      | (up: 57.8 KiB/s, down: 57.8 KiB/s)     |
|  6 | HSPDA         | High-Speed Downlink Packet Access          | (up: 703.1 KiB/s, down: 1706.5 KiB/s)  |
|  7 | LTE           |                                            | (up: 7080.1 KiB/s, down: 21118.2 KiB/s)|
|  8 | EVDO          |                                            | (up: 9155.3 KiB/s, down: 34179.7 KiB/s)|
|  9 | Full          | 以计算机允许的最大速度传输数据。               | |


###### 参考

- [安卓模拟器设置网速和延迟](https://blog.csdn.net/crazyman2010/article/details/53229520)
- [AVD 属性](https://developer.android.com/studio/run/managing-avds.html)

### Absolute path are not supported when setting an output file name

- 解决办法: 修改路径。

    `BEFORE gradle 3.1.0`
```java

applicationVariants.all { variant ->
    variant.outputs.all { output ->
        outputFileName = new File(
                output.outputFile.parent,
                output.outputFile.name)
    }
}
applicationVariants.all { variant ->
    variant.outputs.all { output ->
        outputFileName = new File(
                "./../../../../../build/",
                output.outputFile.name)
    }
}
```
    `IN or AFTER gradle 3.1.0`
```java
applicationVariants.all { variant ->
    variant.outputs.all { output ->
        outputFileName = new File(
                "./../../../../../build/",
                output.outputFile.name)
    }
}
```

######  参考

- [stackoverflow](https://stackoverflow.com/questions/49530142/android-studio-3-1-buildgradle3-1-0-absolute-path-are-not-supported-when-se)

### GreenDao 报错 `org.eclipse.jdt.internal.compiler.impl.CompilerOptions.versionToJdkLevel(Ljava/lang/Object;)J'.`

- 解决办法：

    - 移除app里面build.gradle关于GreenDao的配置

    ```java
    buildscript {
        repositories {
            mavenCentral()
        }
        dependencies {
            classpath 'org.greenrobot:greendao-gradle-plugin:3.2.0'
        }
    }
    ```
    - 在project的build.gradle的`dependencies`里面添加`classpath 'org.greenrobot:greendao-gradle-plugin:3.2.0'`
    ```java
        dependencies {
            classpath 'com.android.tools.build:gradle:2.3.1'
            classpath 'org.greenrobot:greendao-gradle-plugin:3.2.0'
            // NOTE: Do not place your application dependencies here; they belong
            // in the individual module build.gradle files
        }
    ```
### Android Studio 不停的Indexing

- 使用万能的`File > Invalidate Caches/Restart`

### Gradle 打包 错误 `Error:trouble processing "javax/xml/namespace/QName.class":`

```java
Error:trouble processing "javax/xml/namespace/QName.class":
Error:Ill-advised or mistaken usage of a core class (java.* or javax.*)
Error:when not building a core library...
```

- 解决办法：在`build.gradle`的`dexOptions`添加`additionalParameters = ['--core-library']`

```java
    dexOptions {
        javaMaxHeapSize "4g"
        additionalParameters = ['--core-library']
    }
```

