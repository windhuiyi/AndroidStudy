Flutter 安装
==========
### 下载、安装、检测

#### [Get Started: Install](https://flutter.io/get-started/install/)

- 根据网页指导下载、解压、设置路径。


#### 运行`flutter doctor`检测。

```
bogon:~ windhuiyi$ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel beta, v0.5.1, on Mac OS X 10.13.1 17B1003, locale zh-Hans-CN)
[!] Android toolchain - develop for Android devices (Android SDK 27.0.3)
    ! Some Android licenses not accepted.  To resolve this, run: flutter doctor --android-licenses
[!] iOS toolchain - develop for iOS devices (Xcode 9.2)
    ✗ libimobiledevice and ideviceinstaller are not installed. To install, run:
        brew install --HEAD libimobiledevice
        brew install ideviceinstaller
    ✗ ios-deploy not installed. To install:
        brew install ios-deploy
    ! CocoaPods out of date (1.5.0 is recommended).
        CocoaPods is used to retrieve the iOS platform side's plugin code that responds to your plugin usage on the Dart side.
        Without resolving iOS dependencies with CocoaPods, plugins will not work on iOS.
        For more info, see https://flutter.io/platform-plugins
      To upgrade:
        brew upgrade cocoapods
        pod setup
[✓] Android Studio
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[✓] Android Studio
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[✓] Android Studio (version 3.1)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] IntelliJ IDEA Ultimate Edition (version 2017.1.4)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] VS Code (version 1.11.1)
[✓] Connected devices (2 available)
```

##### Android toolchain 问题

- 运行`flutter doctor --android-licenses`解决第一个问题。成功后提示。

```
All SDK package licenses accepted
```

##### iOS toolchain 问题 这个比较坑爹

- 运行`brew install --HEAD libimobiledevice` 安装，不知道成不成功。。。

```
==> Installing dependencies for libimobiledevice: autoconf, automake, libtool, libxml2, libtasn1, libplist, libusb, usbmuxd
Error: Calling keg_only :provided_until_xcode43 is disabled!
There is no replacement.
/usr/local/Homebrew/Library/Homebrew/formula.rb:1035:in `keg_only?'
```

-  接续运行`brew install ideviceinstaller` 安装

```
Please wait for it to finish or terminate it to continue.
==> Installing dependencies for ideviceinstaller: libtasn1, libplist, libusb, usbmuxd, openssl, libimobiledevice, libzip
==> Installing ideviceinstaller dependency: libtasn1
==> Downloading https://homebrew.bintray.com/bottles/libtasn1-4.13.high_sierra.b
######################################################################## 100.0%
==> Pouring libtasn1-4.13.high_sierra.bottle.tar.gz
🍺  /usr/local/Cellar/libtasn1/4.13: 59 files, 435KB
==> Installing ideviceinstaller dependency: libplist
==> Downloading https://homebrew.bintray.com/bottles/libplist-2.0.0.high_sierra.
######################################################################## 100.0%
==> Pouring libplist-2.0.0.high_sierra.bottle.tar.gz
🍺  /usr/local/Cellar/libplist/2.0.0: 31 files, 768.6KB
==> Installing ideviceinstaller dependency: libusb
==> Downloading https://homebrew.bintray.com/bottles/libusb-1.0.22.high_sierra.b
######################################################################## 100.0%
==> Pouring libusb-1.0.22.high_sierra.bottle.tar.gz
🍺  /usr/local/Cellar/libusb/1.0.22: 29 files, 514.8KB
==> Installing ideviceinstaller dependency: usbmuxd
==> Downloading https://homebrew.bintray.com/bottles/usbmuxd-1.0.10_1.high_sierr
######################################################################## 100.0%
==> Pouring usbmuxd-1.0.10_1.high_sierra.bottle.tar.gz
🍺  /usr/local/Cellar/usbmuxd/1.0.10_1: 13 files, 120KB
==> Installing ideviceinstaller dependency: openssl
==> Downloading https://homebrew.bintray.com/bottles/openssl-1.0.2p.high_sierra.
######################################################################## 100.0%
==> Pouring openssl-1.0.2p.high_sierra.bottle.tar.gz
==> Caveats
A CA file has been bootstrapped using certificates from the SystemRoots
keychain. To add additional certificates (e.g. the certificates added in
the System keychain), place .pem files in
  /usr/local/etc/openssl/certs

and run
  /usr/local/opt/openssl/bin/c_rehash

This formula is keg-only, which means it was not symlinked into /usr/local,
because Apple has deprecated use of OpenSSL in favor of its own TLS and crypto libraries.

If you need to have this software first in your PATH run:
  echo 'export PATH="/usr/local/opt/openssl/bin:$PATH"' >> ~/.bash_profile

For compilers to find this software you may need to set:
    LDFLAGS:  -L/usr/local/opt/openssl/lib
    CPPFLAGS: -I/usr/local/opt/openssl/include
For pkg-config to find this software you may need to set:
    PKG_CONFIG_PATH: /usr/local/opt/openssl/lib/pkgconfig

==> Summary
🍺  /usr/local/Cellar/openssl/1.0.2p: 1,793 files, 12.3MB
==> Installing ideviceinstaller dependency: libimobiledevice
==> Downloading https://homebrew.bintray.com/bottles/libimobiledevice-1.2.0_2.hi
######################################################################## 100.0%
==> Pouring libimobiledevice-1.2.0_2.high_sierra.bottle.tar.gz
🍺  /usr/local/Cellar/libimobiledevice/1.2.0_2: 66 files, 984.6KB
==> Installing ideviceinstaller dependency: libzip
==> Downloading https://homebrew.bintray.com/bottles/libzip-1.5.1.high_sierra.bo
######################################################################## 100.0%
==> Pouring libzip-1.5.1.high_sierra.bottle.tar.gz
🍺  /usr/local/Cellar/libzip/1.5.1: 134 files, 577KB
==> Installing ideviceinstaller
==> Downloading https://homebrew.bintray.com/bottles/ideviceinstaller-1.1.0_4.hi
######################################################################## 100.0%
==> Pouring ideviceinstaller-1.1.0_4.high_sierra.bottle.tar.gz
🍺  /usr/local/Cellar/ideviceinstaller/1.1.0_4: 8 files, 64.5KB
==> Caveats
==> openssl
A CA file has been bootstrapped using certificates from the SystemRoots
keychain. To add additional certificates (e.g. the certificates added in
the System keychain), place .pem files in
  /usr/local/etc/openssl/certs

and run
  /usr/local/opt/openssl/bin/c_rehash

This formula is keg-only, which means it was not symlinked into /usr/local,
because Apple has deprecated use of OpenSSL in favor of its own TLS and crypto libraries.

If you need to have this software first in your PATH run:
  echo 'export PATH="/usr/local/opt/openssl/bin:$PATH"' >> ~/.bash_profile

For compilers to find this software you may need to set:
    LDFLAGS:  -L/usr/local/opt/openssl/lib
    CPPFLAGS: -I/usr/local/opt/openssl/include
For pkg-config to find this software you may need to set:
    PKG_CONFIG_PATH: /usr/local/opt/openssl/lib/pkgconfig
```

- 运行`brew install ios-deploy`安装

```
==> Downloading https://homebrew.bintray.com/bottles/ios-deploy-1.9.2.high_sierr
######################################################################## 100.0%
==> Pouring ios-deploy-1.9.2.high_sierra.bottle.tar.gz
🍺  /usr/local/Cellar/ios-deploy/1.9.2: 7 files, 153.8KB
```

- 运行`brew upgrade cocoapods` 提示

```
Error: cocoapods not installed
```

- 只好运行`brew install cocoapods`安装

```
==> Downloading https://homebrew.bintray.com/bottles/cocoapods-1.5.3.high_sierra
######################################################################## 100.0%
==> Pouring cocoapods-1.5.3.high_sierra.bottle.tar.gz
Error: The `brew link` step did not complete successfully
The formula built, but is not symlinked into /usr/local
Could not symlink bin/pod
Target /usr/local/bin/pod
already exists. You may want to remove it:
  rm '/usr/local/bin/pod'

To force the link and overwrite all conflicting files:
  brew link --overwrite cocoapods

To list all files that would be deleted:
  brew link --overwrite --dry-run cocoapods

Possible conflicting files are:
/usr/local/bin/pod
/usr/local/bin/xcodeproj
==> Summary
🍺  /usr/local/Cellar/cocoapods/1.5.3: 8,923 files, 13.2MB
```

- 运行`pod setup`，出现问题`Setting up CocoaPods master repo `会卡住，谷歌一下说GEM 的地址问题。

- 这时候GEM 的版本又太低了。。。先升级GEM,运行`sudo gem update --system`

```
RubyGems installed the following executables:
	/usr/local/Cellar/ruby/2.2.2/bin/gem
	/usr/local/Cellar/ruby/2.2.2/bin/bundle

Ruby Interactive (ri) documentation was installed. ri is kind of like man 
pages for Ruby libraries. You may access it like this:
  ri Classname
  ri Classname.class_method
  ri Classname#instance_method
If you do not wish to install this documentation in the future, use the
--no-document flag, or set it as the default in your ~/.gemrc file. See
'gem help env' for details.

RubyGems system software updated

```
- 升级完成后，运行`gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/`更换地址。之后运行gem sources -l`查看是否更新成功。
```
bogon:~ windhuiyi$ gem sources -l
*** CURRENT SOURCES ***

https://gems.ruby-china.com/
```

- 之后还运行`sudo rm -fr ~/.cocoapods/repos/master` 和 `pod repo remove master` 删除本地的 master ，之后再运行`pod setup`不过扑街要等很久。。。


```
CocoaPods 1.6.0.beta.1 is available.
To update use: `sudo gem install cocoapods --pre`
[!] This is a test version we'd love you to try.

For more information, see https://blog.cocoapods.org and the CHANGELOG for this version at https://github.com/CocoaPods/CocoaPods/releases/tag/1.6.0.beta.1

Setup completed
```

- 说这个`CocoaPods`太老了，需要更新`sudo gem install cocoapods --pre`，更新之后再运行`pod setup`

```
/usr/local/lib/ruby/gems/2.2.0/gems/cocoapods-1.6.0.beta.1/lib/cocoapods/executable.rb:89: warning: Insecure world writable dir /usr/local/bin in PATH, mode 040777
Setting up CocoaPods master repo
  $ /usr/local/bin/git -C /Users/windhuiyi/.cocoapods/repos/master fetch origin
  --progress
  remote: Counting objects: 18, done.        
  remote: Compressing objects: 100% (17/17), done.        
  remote: Total 18 (delta 10), reused 0 (delta 0), pack-reused 0        
  From https://github.com/CocoaPods/Specs
     5a2dffa..47b7fd4  master     -> origin/master
  $ /usr/local/bin/git -C /Users/windhuiyi/.cocoapods/repos/master rev-parse
  --abbrev-ref HEAD
  master
  $ /usr/local/bin/git -C /Users/windhuiyi/.cocoapods/repos/master reset --hard
  origin/master
  HEAD is now at 47b7fd4 [Add] ShoukoShareUI 0.0.2
Setup completed
```

- 搞定之后再运行`flutter doctor`，这样`Android toolchain`和`iOS toolchain`都搞定了。

```
bogon:.cocoapods windhuiyi$ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel beta, v0.5.1, on Mac OS X 10.13.1 17B1003, locale zh-Hans-CN)
[✓] Android toolchain - develop for Android devices (Android SDK 27.0.3)
[✓] iOS toolchain - develop for iOS devices (Xcode 9.2)
[✓] Android Studio
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[✓] Android Studio
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[✓] Android Studio (version 3.1)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] IntelliJ IDEA Ultimate Edition (version 2017.1.4)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] VS Code (version 1.11.1)
[✓] Connected devices (1 available)

! Doctor found issues in 2 categories.
```

#### Android Studio 问题

- Android Studio 安装`Flutter`和`Dart`才有用，Android Studio Preview 安装没有用。。。