### Dragger 2 使用汇总

### ### dagger2 在组件化使用

- 当组件为 app 时，可以Component单独注入。

```java
            DaggerAppComponent.builder().application(application).build().inject(application);

```
- 当组件为library 时，不能各个组件Component单独注入了，需要在app 的 module 里面，包含所有的 module，然后在 app 的Component注入。不然会报错。因为单独注入的话，map 不能包含全部。
