Gradle Bug 错误汇总
==============================

[apk does not exist on disk.](#apk-does-not-exist-on-disk)  
[All flavors must now belong to a named flavor dimension.](#All-flavors-must-now-belong-to-a-named-flavor-dimension)  
[gradlew: command not found](#gradlew-command-not-found)  
[Absolute path are not supported when setting an output file name](#Absolute-path-are-not-supported-when-setting-an-output-file-name)  
[GreenDao 报错 `org.eclipse.jdt.internal.compiler.impl.CompilerOptions.versionToJdkLevel(Ljava/lang/Object;)J'.`](#orgeclipsejdtinternalcompilerimplCompilerOptionsversionToJdkLevelLjavalangObjectJ)  
[Error:trouble processing "javax/xml/namespace/QName.class"](#Errortrouble-processing-javaxxmlnamespaceQNameclass)  
[Invalid main APK outputs : BuildOutput](#Invalid-main-APK-outputs--BuildOutput)  
[Cause: duplicate entry: org/intellij/lang/annotations/Flow.class](#Cause-duplicate-entry-orgintellijlangannotationsFlowclass)  
["Error converting bytecode to dex","Translation has been interrupted"](#Error-converting-bytecode-to-dexTranslation-has-been-interrupted)  
[uses-sdk:minSdkVersion  cannot be smaller than version  declared in library](#uses-sdkminSdkVersion--cannot-be-smaller-than-version--declared-in-library)  
[Gradle compile 依赖包下载位置](#Gradle-compile-依赖包下载位置)  
[Gradle compile 没有下载](#Gradle-compile-没有下载)  
[Gradle compile api implementation 区别](#Gradle-compile-api-implementation-区别)  
[No-toolchains-found-in-the-NDK-toolchains-folder](#No-toolchains-found-in-the-NDK-toolchains-folder)
[Gradle 在library module 的signingConfigs不起作用](#Gradle-在library-module-的signingConfigs不起作用)  
[Gradle Libraly Module switch 语句报错](#gradle-libraly-module-switch-语句报错)
[Program type already present](#program-type-already-present)

### apk does not exist on disk.

- 解决办法：点一下`Gradle projects`的那个刷新图标就行了。

![android_studio_gradle_01](/images/android_studio_gradle_01.png "android_studio_gradle_01")

### All flavors must now belong to a named flavor dimension.

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

### gradlew: command not found

- 解决办法：不要运行`gradlew`,运行`./gradlew`,不过一开始还是不行的。。。需要先运行`chmod ./gradlew`


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

### org.eclipse.jdt.internal.compiler.impl.CompilerOptions.versionToJdkLevel(Ljava/lang/Object;)J'.

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


### Error:trouble processing "javax/xml/namespace/QName.class":

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

### Invalid main APK outputs : BuildOutput

- 解决办法:在`lintOptions`里面添加`checkReleaseBuilds`为 false

```java
lintOptions { 

    checkReleaseBuilds false

}
```

### Cause: duplicate entry: org/intellij/lang/annotations/Flow.class

- 解决方法：在 android 下添加

```java
    configurations {
        compile.exclude group:'org.jetbrains', module: 'annotations'
    }
```

###### 参考

[How to fix Gradle build error: “java.util.zip.ZipException: duplicate entry”](https://medium.com/@terrakok/how-to-fix-gradle-build-error-java-util-zip-zipexception-duplicate-entry-83d102038056)

### "Error converting bytecode to dex","Translation has been interrupted"

- 一般存在包冲突，需要寻找到冲突的包。


### uses-sdk:minSdkVersion  cannot be smaller than version  declared in library

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

### No toolchains found in the NDK toolchains folder

- 具体报错信息
```
No toolchains found in the NDK toolchains folder for ABI with prefix: mips64el-linux-android This version of the NDK may be incompatible with the Android Gradle plugin version 3.0 or older.
```
- 解决版本：下载新的 NDK 包，解压。在`toolchains`文件夹下面找到`mips64el-linux-android-4.9`把它复制进原来的`toolchains`文件夹下，这样就不会报错了。



### Gradle Libraly Module switch 语句报错

- Resource IDs cannot be used in a switch statement in Android library modules  因为id 不是 final int 了。要用 if-else 语句了。



### Program type already present

```java
com.android.tools.r8.errors.CompilationError: Program type already present
```

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

### 方法数超过64K

- 报错-Cannot fit requested classes in a single dex file (# methods: 68106 > 65536)

- minSdkVersion 设置为 21 或更高值，只需设置multiDexEnabled为 true
```java
android {
    defaultConfig {
        ...
        minSdkVersion 21
        targetSdkVersion 28
        multiDexEnabled true
    }
    ...
}
```
- 如果您的 minSdkVersion 设置为 20 或更低值
```java
android {
    defaultConfig {
        ...
        minSdkVersion 15
        targetSdkVersion 28
        multiDexEnabled true // 启用
    }
    ...
}

dependencies {
  compile 'com.android.support:multidex:1.0.3' // 依赖
}

public class MyApplication extends MultiDexApplication { ... } // 继承
```

###### 参考

[配置方法数超过 64K 的应用](https://developer.android.com/studio/build/multidex)

