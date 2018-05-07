使用WebViewV遇到问题总结
================

[远程调试WebView](#远程调试WebView)  
[WebView加载URL跳转到系统浏览器的问题](#WebView加载URL跳转到系统浏览器的问题)  
[WebView 中文乱码](#WebView-中文乱码)  
[WebView JS 交互](#WebVie-JS-交互)  
[WebView 显示错误](#WebView-显示错误)

### 远程调试WebView

#### 配置 WebViews 进行调试

- 比较强大，可以查看源码，查看错误等，用于调试，排错。

```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
    WebView.setWebContentsDebuggingEnabled(true);
}
```

#### 在 DevTools 中打开 WebView

chrome://inspect 页面将显示您的设备上已启用调试的 WebView 列表。  
要开始调试，请点击您想要调试的 WebView 下方的 inspect。像使用远程浏览器标签一样使用 DevTools

### WebView加载URL跳转到系统浏览器的问题

- 设置`WebViewClient`,重写`shouldOverrideUrlLoading`方法
``` java
webView.setWebViewClient(new WebViewClient(){
@Override
public boolean shouldOverrideUrlLoading(WebView view, String url) {
    view.loadUrl(url);
    return super.shouldOverrideUrlLoading(view, url);
    }
});
```

###### 参考文章
- 解决WebView加载URL跳转到系统浏览器的问题](https://blog.csdn.net/yy1300326388/article/details/43965493)


### WebView 中文乱码

- `loadData`的时候，设置`mimeType`为`"text/html; charset=UTF-8"`

```java
 loadData(data, "text/html; charset=UTF-8", null);
```

### WebView JS 交互

#### Enabling JavaScript

```java
WebView myWebView = (WebView) findViewById(R.id.webview);
WebSettings webSettings = myWebView.getSettings();
webSettings.setJavaScriptEnabled(true);
```

#### Binding JavaScript code to Android code

- 定义JS接口，关键是和html的javascript名称保持一致！～

```java
// 定义app实现JS接口
public class WebAppInterface {
    Context mContext;

    /** Instantiate the interface and set the context */
    WebAppInterface(Context c) {
        mContext = c;
    }

    /** Show a toast from the web page */
    // 也可以直接定义在Activity里面
    @JavascriptInterface
    public void showToast(String toast) {
        Toast.makeText(mContext, toast, Toast.LENGTH_SHORT).show();
    }
}
```

- WebView添加JS接口

```java
WebView webView = (WebView) findViewById(R.id.webview);
// 添加JS接口
webView.addJavascriptInterface(new WebAppInterface(this), "Android");
// @JavascriptInterface 定义在Activity里面，使用这个
webView.addJavascriptInterface(this, "Android");
```

- html的JS接口定义

```html
<!-- 保持一致才能起作用 -->
<input type="button" value="Say hello" onClick="showAndroidToast('Hello Android!')" />

<script type="text/javascript">
    function showAndroidToast(toast) {
        Android.showToast(toast);
    }
</script>
```

###### 参考文章

- [Building Web Apps in WebView](https://developer.android.com/guide/webapps/webview)

### WebView 显示错误

- 可以远程调试WebView，排查错误

```java
// 网上一般说加上这些。不过有没有用靠运气，不行要调试看看。
// 开启DomStorage
web.getSettings().setDomStorageEnabled(true);
// 使用viewport
web.getSettings().setUseWideViewPort(true);
// 采用OverviewMode
web.getSettings().setLoadWithOverviewMode(true);
web.getSettings().setLayoutAlgorithm(WebSettings.LayoutAlgorithm.SINGLE_COLUMN);
```


