使用WebViewV遇到问题总结
================

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

