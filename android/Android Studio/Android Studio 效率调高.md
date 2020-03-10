Android Studio 效率调高
==========
### 快捷键使用

- 折叠/展开代码块（Collapse Expand Code Block） Cmd + “+”/”-“(OS X)
  - 该操作提供一种方法，让你隐藏你不关心的部分代码，以一种较为简洁的格式显示关键代码。一个有意思的用法是隐藏匿名内部类的代码，让其看起来像一个Lambda表达式。

-  定位到嵌套文件（Navigate to Nested File）
  - 有时你有一堆存放在不同目录下的同名文件，例如不同模块下的`AndroidManifest.xml`文件，当你想定位到其中的一个文件，你会得到一堆搜索结果，你还得辨认哪个才是你需要的。通过在检索框中输入部分路径的前缀，并添加斜杠号，你就可以在第一次尝试的时候就找到正确的那个。输入 app/mani就可以搜索到 app module 下的AndroidManifest.xml

- 快速查看定义（Quick Definition Lookup）Cmd + Y(OS X)
  - 你曾经是否想查看一个方法或者类的具体实现，但是不想离开当前界面？ 该操作可以帮你搞定。

-  提取方法（Extract Method） Cmd + Alt + M(OS X)
  - 提取一段代码块，生成一个新的方法。当你发现某个方法里面过于复杂，需要将某一段代码提取成单独的方法时，该技巧是很有用的。

### Postfix Completion 后缀补全

- list 应用
```java
        // list.for 生成
        for (String s : list) {
            
        }
        // list.fori 生成
        for (int i = 0; i < list.size(); i++) {
            
        }
        // list.forr 生成
        for (int i = list.size() - 1; i >= 0; i--) {
            
        }
```

- 空、非空判断

```java
        // list.null 生成
        if (list == null) {

        }
        // list.nn 生成
        if (list != null) {

        }
```

- 条件判断

```java
        // list.size() > 5.if 生成
        if (list.size() > 5) {

        }
```

- 异常捕获

```java
        // list.try 生成
        try {
            list
        } catch (Exception e) {
            e.printStackTrace();
        }
```

### Android studio 过滤日志
- 在A ndroid Studio Log 那里添加过滤，在LogTag 那里输入下面的正则表达式。
```java
^(?!.*(xxx|yyy)).*$  // 正则表达式,|进行分割
```