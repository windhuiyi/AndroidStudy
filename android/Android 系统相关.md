Android 系统相关
=======

### Android 获取手机相关信息

```java
     /**
     * 获取当前手机系统语言。
     */
    public static String getSystemLanguage() {
        return Locale.getDefault().getLanguage();
    }
 
    /**
     * 获取当前系统上的语言列表(Locale列表)
     */
    public static Locale[] getSystemLanguageList() {
        return Locale.getAvailableLocales();
    }
 
    /**
     * 获取当前手机系统版本号
     */
    public static String getSystemVersion() {
        return android.os.Build.VERSION.RELEASE;
    }
 
    /**
     * 获取手机型号
     */
    public static String getSystemModel() {
        return android.os.Build.MODEL;
    }
 
    /**
     * 获取手机厂商
     */
    public static String getDeviceBrand() {
        return android.os.Build.BRAND;
    }
 
    /**
     * 获取手机IMEI(需要“android.permission.READ_PHONE_STATE”权限)
     */
    public static String getIMEI(Context ctx) {
        TelephonyManager tm = (TelephonyManager) ctx.getSystemService(Activity.TELEPHONY_SERVICE);
        if (tm != null) {
            return tm.getDeviceId();
        }
        return null;
    }
```

### Android 获取进程、内存信息

```java
int pid = android.os.Process.myPid(); // 获取 pid

ActivityManager activityManager = (ActivityManager) getSystemService(ACTIVITY_SERVICE);
ActivityManager.MemoryInfo info = new ActivityManager.MemoryInfo();
activityManager.getMemoryInfo(info);
 
 
Log.i(TAG, "系统剩余内存:" + (info.availMem >> 20) + "MB");
Log.i(TAG, "系统总内存:" + (info.totalMem >> 20)+"MB");
Log.i(TAG, "系统是否处于低内存运行：" +info.lowMemory);
Log.i(TAG, "当系统剩余内存低于" + (info.threshold >> 20) + "MB时就看成低内存运行");
 
 
Runtime rt = Runtime.getRuntime();
 
Log.d(TAG, "Available heap " + (rt.freeMemory() >> 20) + "MB");
Log.d(TAG, "MAX heap " + (rt.maxMemory() >> 20) + "MB");
Log.d(TAG, "totle heap " + (rt.totalMemory() >> 20) + "MB");

```