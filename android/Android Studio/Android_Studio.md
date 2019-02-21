Android Studio 使用汇总
===================

### Android Studio 不停的Indexing

- 使用万能的`File > Invalidate Caches/Restart`

### Android Studio `preview waiting for build to finish`

- just Clean Project

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

### DELETE_FAILED_INTERNAL_ERROR 错误

- 这个是小米手机的问题，需要打开USB安装，这 tmd 需要插入 sim 卡。

### Android Studio 新建类头部注释 设置

- 设置位置 Editor -> File and Code Templates -> Includes -> File Header

![file_header](https://raw.githubusercontent.com/windhuiyi/AndroidStudy/19e5580e591613e69a773c1bd6b2d1c5a9477966/images/file_header.png)

