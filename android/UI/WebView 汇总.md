使用WebViewV遇到问题总结
================

[远程调试WebView](#远程调试webview)  
[WebView加载URL跳转到系统浏览器的问题](#WebView-拦截-URL-不跳转到系统浏览器)  
[WebView 中文乱码](#webview-中文乱码)  
[WebView JS 交互](#webview-js-交互)  
[WebView 显示错误](#webview-显示错误)  
[WebView 报错 ERR_UNKNOWN_URL_SCHEME](#WebView-报错-ERR-UNKNOWN-URL-SCHEME)

### 远程调试WebView

- 配置 WebViews 进行调试

  - 比较强大，可以查看源码，查看错误等，用于调试，排错。

```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
    WebView.setWebContentsDebuggingEnabled(true);
}
```

- 在 DevTools 中打开 WebView

  - chrome://inspect 页面将显示您的设备上已启用调试的 WebView 列表。  
  - 要开始调试，请点击您想要调试的 WebView 下方的 inspect。像使用远程浏览器标签一样使用 DevTools

### WebView 拦截 URL 不跳转到系统浏览器

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
- [解决WebView加载URL跳转到系统浏览器的问题](https://blog.csdn.net/yy1300326388/article/details/43965493)


### WebView 中文乱码

- `loadData`的时候，设置`mimeType`为`"text/html; charset=UTF-8"`

```java
 loadData(data, "text/html; charset=UTF-8", null);
```

### WebView JS 交互

#### 1.Enabling JavaScript

```java
WebView myWebView = (WebView) findViewById(R.id.webview);
WebSettings webSettings = myWebView.getSettings();
webSettings.setJavaScriptEnabled(true);
```

#### 2.Binding JavaScript code to Android code

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

### WebView 内容字体大小

```java
binding.wvAgreement.getSettings().setTextZoom(250);
```

### WebView 按返回键返回上一页

- 改写`onBackPressed`和`onKeyDown`，判断是否能`canGoBack`

```java
// 改写 onBackPressed
@Override
public void onBackPressed() {
    if(mWebView.canGoBack()){
        mWebView.goBack();
        return;
    }

    super.onBackPressed();
}

// 改写onKeyDown
public boolean onKeyDown(int keyCode, KeyEvent event){
    if ((keyCode == KeyEvent.KEYCODE_BACK) && mWebView.canGoBack()) {
        mWebView.goBack();
        return true;
    }

    return super.onKeyDown(keyCode, event);
}
```

### WebView 前进 后退 刷新

```java
mWebView.goBack(); //后退
mWebView.goForward();//前进
mWebView.reload(); //刷新
```

### WebView 加载不出网页 显示错误或者空白

```java
        webView.getSettings().setLayoutAlgorithm(WebSettings.LayoutAlgorithm.SINGLE_COLUMN);
        webView.getSettings().setUseWideViewPort(true);
        webView.getSettings().setLoadWithOverviewMode(true);
        webView.getSettings().setDomStorageEnabled(true);
        webView.getSettings().setAppCacheEnabled(true);
        webView.getSettings().setLoadsImagesAutomatically(true);
        webView.getSettings().setJavaScriptEnabled(true); // 加上这个才显示正确
```

- 还有一个问题就是 WebView的布局问题也可能导致显示不出。

### WebView HTTP 和 HTTPS 请求混合

- 请求的地址如果是HTTPS开头的，如果有 HTTP的请求，这时候会不起作用。。。可能导致HTTP开头的图片显示不出来，或者接口请求不了，需要设置混合模式。
```java
 if (Build.VERSION.SDK_INT > Build.VERSION_CODES.LOLLIPOP){
            bindingView.get().wv.getSettings().setMixedContentMode(WebSettings.MIXED_CONTENT_ALWAYS_ALLOW);
        }
```

### WebView 报错 ERR UNKNOWN URL SCHEME

- 复写`shouldOverrideUrlLoading`方法时，`view.loadUrl(url);`造成的，去除后不会报错。
```java
            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                return super.shouldOverrideUrlLoading(view, url);
            }
```