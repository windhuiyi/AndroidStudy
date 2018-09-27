EditText 汇总
============
### EditText 自定义光标

- 设置`textCursorDrawable`自定义光标

```xml
android:textCursorDrawable="@drawable/cursor"
```

- 自定义 shape

```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <size android:width="1dp" />
    <solid android:color="#000000" />
</shape>
```
### EditText hint 大小

- 可以用 html 格式自定义大小

```xml
<string name="edittext_hint"><font size="15">Hint here!</font></string>
```

- 可以代码设置Html 格式字符串

```java
editText.setHint(Html.fromHtml("<font size=\"5\">" + "hinttext1" + "</font>" 
+ "<small>" + "hinttext2" + "</small>" )); 
```
