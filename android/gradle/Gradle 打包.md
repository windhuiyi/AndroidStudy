Gradle 打包相关知识
=================
#### 开发过程中，有生产环境、测试环境、开发环境等，它们使用的服务器地址和端口可能不同。可以在`gradle`里面进行配置，运行或生成不同配置的app

- 可以配置`buildTypes`

```java
    buildTypes {
        release {
            signingConfig signingConfigs.release
            minifyEnabled false
            //Zipalign优化
            zipAlignEnabled true
            // 移除无用的resource文件
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            buildConfigField "String", "WXURL", "\"https://api.weixin.qq.com/\""
            buildConfigField "String", "URL", "\"https://www.baidu.com\""
        }
        debug {
            signingConfig signingConfigs.debug
            minifyEnabled false
            //Zipalign优化
            zipAlignEnabled true
            // 移除无用的resource文件
            shrinkResources false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            buildConfigField "String", "WXURL", "\"https://api.weixin.qq.com/\""
            buildConfigField "String", "URL", "\"http://www.baidu.com\""            
        }
    }
```

- 可以配置`productFlavors`

> PS:需要先添加`flavorDimensions`,然后在各个flavor里面添加`dimension`,名称保持一致。

```java
    flavorDimensions "test"
    productFlavors {
        sit {
            applicationId "com.test.sit"
            versionName "sit_1.0.1"
            dimension "test"
            manifestPlaceholders = [APP_NAME: "Test_sit"]
        }
        pro {
            applicationId "com.test
            versionName "1.0.5.12"
            dimension "test"
            manifestPlaceholders = [APP_NAME: "Test"]
        }
    }
```

- 这样的话，`buildTypes`和`productFlavors`会进行两两组合，生成多个`Build Variant`。

#### 如何一次生成所有`Release`包

- 使用命令`./gradlew assembleRelease`,如果在Mac下报错，需要先运行`chmod 755 ./gradlew`


##### 参考
---------
[构建神器Gradle](http://jiajixin.cn/2015/08/07/gradle-android/)  
[构建 Variants（变种）版本](https://chaosleong.gitbooks.io/gradle-for-android/content/build_variants/)  
[Android Studio打包全攻略](http://www.cnblogs.com/jiuyi/p/6098589.html)


