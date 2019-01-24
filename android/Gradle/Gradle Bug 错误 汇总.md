Gradle Bug 错误汇总
==============================

[Android Studio Run 报错:`apk does not exist on disk.`](#Android-Studio-Run-报错)
[Gradle 在library module 的signingConfigs不起作用](#Gradle-在library-module-的signingConfigs不起作用)
[Gradle 运行APP报错 com.android.tools.r8.errors.CompilationError: Program type already present](#Gradle-运行APP报错-com.android.tools.r8.errors.CompilationError:-Program-type-already-present)

###   Android Studio Run 报错:`apk does not exist on disk.`

- 解决办法：点一下`Gradle projects`的那个刷新图标就行了。

![android_studio_gradle_01](/images/android_studio_gradle_01.png "android_studio_gradle_01")

### Gradle 报错:`Error:All flavors must now belong to a named flavor dimension.`

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

###  Gradle 使用`gradlew`命令 报错:`gradlew: command not found`

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

### Gradle 报错 `Absolute path are not supported when setting an output file name`

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

### Gradle 打包 错误 `Invalid main APK outputs : BuildOutput`

- 解决办法:在`lintOptions`里面添加`checkReleaseBuilds`为 false

```java
lintOptions { 

    checkReleaseBuilds false

}
```

### Gradle Make Module error "Cause: duplicate entry: org/intellij/lang/annotations/Flow.class"

- 解决方法：在 android 下添加

```java
    configurations {
        compile.exclude group:'org.jetbrains', module: 'annotations'
    }
```

###### 参考

[How to fix Gradle build error: “java.util.zip.ZipException: duplicate entry”](https://medium.com/@terrakok/how-to-fix-gradle-build-error-java-util-zip-zipexception-duplicate-entry-83d102038056)

### Gradle Run Error "Error converting bytecode to dex","Translation has been interrupted"

- 一般存在包冲突，需要寻找到冲突的包。

### Android Studio `preview waiting for build to finish`

- just Clean Project

### Run 错误 `uses-sdk:minSdkVersion  cannot be smaller than version  declared in library`

- 在`AndroidManifest.xml`添加

```xml
<uses-sdk tools:overrideLibrary="com.chad.library" />
```

### Gradle compile 依赖包下载位置

- MAC: `~/.gradle/caches/modules-2/files-2.1`

### Gradle compile 没有下载

- 只能下载 jar 包，或者 compile aar 包。

### Gradle compile api implementation 区别

- compile和 api 没有区别， api代替之前的compile

- implementation只作用于当前 module，不会传递。例如 AModule implementation Glide,Glide 只在 AModule 起作用，BModule 虽然依赖 AModule，但是无法引用 Glide。

- 依赖首先应该设置为`implementation`的，如果没有错，那就用`implementation`，如果有错，那么使用`api`指令。使用`implementation`会使编译速度有所增快。

### Gradle Error `No toolchains found in the NDK toolchains folder `

- 具体报错信息
```
No toolchains found in the NDK toolchains folder for ABI with prefix: mips64el-linux-android This version of the NDK may be incompatible with the Android Gradle plugin version 3.0 or older.
```
- 解决版本：下载新的 NDK 包，解压。在`toolchains`文件夹下面找到`mips64el-linux-android-4.9`把它复制进原来的`toolchains`文件夹下，这样就不会报错了。



### Gradle Libraly Module switch 语句 如果是 view.getId() 会报错

- Resource IDs cannot be used in a switch statement in Android library modules 因为id 不是 final int 了。要用 if-else 语句了。



### Gradle 运行APP报错 `com.android.tools.r8.errors.CompilationError: Program type already present`

- 其实就是这个类已经加载或存在了,也就是说很大的可能是因为重复引入了这个类,所以就去检查了这个类都存在哪些jar包中,最后在引用里发现这两个引用里面都有这个类,所以这个问题去掉一个就解决了,当遇到这个问题的时候可以检查下jar包有没有重复的

### Gradle 在library module 的`signingConfigs`不起作用。

- 需要设置在application module 的build.gradle 里面

```java
    signingConfigs {
        release {
            keyAlias 
            keyPassword ''
            storeFile file('')
            storePassword ''
        }

    }
    buildTypes {
        release {           
            signingConfig signingConfigs.release
        }
        debug {
            signingConfig signingConfigs.release
        }
    }
```