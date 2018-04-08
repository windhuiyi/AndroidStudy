应用内启动Activity
=============
#### 应用内启动Activity流程图
![activity_start_flow](/images/activity_start_flow.png "应用内启动Activity流程")
-------------
1 Activity会执行的`startActivity`有几种重载方法，但最终会调用`startActivityForResult`

2 Activity 的` startActivityForResult`会调用Instrumentation的`execStartActivity`方法
```java
Instrumentation.ActivityResult ar =
                 mInstrumentation.execStartActivity(
                     this, mMainThread.getApplicationThread(), mToken, this,
                     intent, requestCode, options);
```
3 Instrumentation的`execStartActivity`方法会调用 ActivityManagerNative的`startActivity`
```java
int result = ActivityManagerNative.getDefault()
                 .startActivity(whoThread, who.getBasePackageName(), intent,
                         intent.resolveTypeIfNeeded(who.getContentResolver()),
                         token, target != null ? target.mEmbeddedID : null,
                         requestCode, 0, null, options);
```
> ps:这个方法最后还会执行`checkStartActivityResult`来检查是否启动成功。

4 ActivityManagerNative的`gDefault`会通过Binder返回ActivityManagerService的单例。实际上是调用的是ActivityManagerService的` startActivity `方法，会调用`startActivityAsUser`
```java
startActivityAsUser(caller, callingPackage, intent, resolvedType, resultTo,
             resultWho, requestCode, startFlags, profilerInfo, options,
             UserHandle.getCallingUserId());
5 ActivityManagerService的` startActivityAsUser `会调用ActivityStackSupervisor的` startActivityMayWait `
mStackSupervisor.startActivityMayWait(caller, -1, callingPackage, intent,
                 resolvedType, null, null, resultTo, resultWho, requestCode, startFlags,
                 profilerInfo, null, null, options, userId, null, null);
```
6 ActivityStackSupervisor的` startActivityMayWait `会调用`startActivityLocked`
```java
int res = startActivityLocked(caller, intent, resolvedType, aInfo,
                     voiceSession, voiceInteractor, resultTo, resultWho,
                     requestCode, callingPid, callingUid, callingPackage,
                     realCallingPid, realCallingUid, startFlags, options,
                     componentSpecified, null, container, inTask);
```
7 ActivityStackSupervisor的`startActivityLocked`会调用`startActivityUncheckedLocked`
```java
err = startActivityUncheckedLocked(r, sourceRecord, voiceSession, voiceInteractor,
                 startFlags, true, options, inTask);  
```
8 ActivityStackSupervisor的` startActivityUncheckedLocked `会调用ActivityStack的`startActivityLocked`
```java
targetStack.startActivityLocked(r, newTask, doResume, keepCurTransition, options);  
```
9 ActivityStack的` startActivityLocked `会ActivityStackSupervisor调用的`resumeTopActivitiesLocked`
```java
mStackSupervisor.resumeTopActivitiesLocked(this, r, options);
```
10 ActivityStackSupervisor的` resumeTopActivitiesLocked `又会调用回ActivityStack的`resumeTopActivityLocked`
```java
 result = targetStack.resumeTopActivityLocked(target, targetOptions);
```
11 ActivityStack的` resumeTopActivityLocked `又会调用自身的` resumeTopActivityInnerLocked `
```java
result = resumeTopActivityInnerLocked(prev, options);
```
12 ActivityStack的` resumeTopActivityInnerLocked `会调用自身的` startPausingLocked `先暂停之前的Activity
```java
pausing |= startPausingLocked(userLeaving, false, true, dontWaitForPause);
```
> PS： 这里会结束掉这个resumeTopActivityInnerLocked，返回true

```java
if (pausing) {
             if (DEBUG_SWITCH || DEBUG_STATES) Slog.v(TAG,
                     "resumeTopActivityLocked: Skip resume: need to start pausing");
             // At this point we want to put the upcoming activity's process
             // at the top of the LRU list, since we know we will be needing it
             // very soon and it would be a waste to let it get killed if it
             // happens to be sitting towards the end.
             if (next.app != null && next.app.thread != null) {
                 mService.updateLruProcessLocked(next.app, true, null);
             }
             if (DEBUG_STACK) mStackSupervisor.validateTopActivitiesLocked();
             return true;
         }  
```
13  ActivityStack的` startPausingLocked `会调用ActivityRecord的ProcessRecord
的IApplicationThread，通过Binder实际上调用ActivityThread的ApplicationThread的`schedulePauseActivity`
```java
prev.app.thread.schedulePauseActivity(prev.appToken, prev.finishing,
                         userLeaving, prev.configChangeFlags, dontWait);
```
14 ActivityThread的ApplicationThread的`schedulePauseActivity`发送消息最后会调用ActivityThread的` handlePauseActivity `,最后会调用ActivityManagerService的` activityPaused `
> PS:performPauseActivity会调用Activity的onPause

```java
ActivityManagerNative.getDefault().activityPaused(token);
```
15 ActivityManagerService的` activityPaused `会调用ActivityStack的`activityPausedLocked`
```java
 stack.activityPausedLocked(token, false);
```
16 用ActivityStack的`activityPausedLocked`会调用自身的` completePauseLocked `
```java
completePauseLocked(true);
```
17 用ActivityStack的` completePauseLocked `又会调用ActivityStackSupervisor的` resumeTopActivitiesLocked `
```java
mStackSupervisor.resumeTopActivitiesLocked(topStack, prev, null);
```
18 ActivityStackSupervisor的` resumeTopActivitiesLocked `又会调用回ActivityStack的`resumeTopActivityLocked`
```java
 result = targetStack.resumeTopActivityLocked(target, targetOptions);
```
19 ActivityStack的` resumeTopActivityLocked `又会调用自身的` resumeTopActivityInnerLocked `
```java
result = resumeTopActivityInnerLocked(prev, options);
```
20 ActivityStack的` resumeTopActivityInnerLocked `会调用ActivityStackSupervisor的` startSpecificActivityLocked `
```java
if (next.app != null && next.app.thread != null) {
 ...
}else{
mStackSupervisor.startSpecificActivityLocked(next, true, true);
}  
```
21 ActivityStackSupervisor的` startSpecificActivityLocked `,因为应用程序已经运行，会调用ActivityStackSupervisor的` realStartActivityLocked `
```java
        // Is this activity's application already running?
        ProcessRecord app = mService.getProcessRecordLocked(r.processName,
                r.info.applicationInfo.uid, true);

        r.task.stack.setLaunchTime(r);
        if (app != null && app.thread != null) {
            try {
                if ((r.info.flags&ActivityInfo.FLAG_MULTIPROCESS) == 0
                        || !"android".equals(r.info.packageName)) {
                    // Don't add this if it is a platform component that is marked
                    // to run in multiple processes, because this is actually
                    // part of the framework so doesn't make sense to track as a
                    // separate apk in the process.
                    app.addPackage(r.info.packageName, r.info.applicationInfo.versionCode,
                            mService.mProcessStats);
                }
                realStartActivityLocked(r, app, andResume, checkConfig);
                return;
            } catch (RemoteException e) {
                Slog.w(TAG, "Exception when starting activity "
                        + r.intent.getComponent().flattenToShortString(), e);
            }

            // If a dead object exception was thrown -- fall through to
            // restart the application.
        }
```
22 ActivityStackSupervisor的` realStartActivityLocked `会调用ActivityThread的ApplicationThread的` scheduleLaunchActivity `
```java
			app.thread.scheduleLaunchActivity(new Intent(r.intent), r.appToken,
                     System.identityHashCode(r), r.info, new Configuration(mService.mConfiguration),
                     r.compat, r.launchedFromPackage, r.task.voiceInteractor, app.repProcState,
                     r.icicle, r.persistentState, results, newIntents, !andResume,
```
23 ApplicationThread的` scheduleLaunchActivity `最终会调用ActivityThread的`handleLaunchActivity`

24 ActivityThread的`handleLaunchActivity`会调用自身的`performLaunchActivity`
```java
Activity a = performLaunchActivity(r, customIntent);
```
25 ActivityThread的` performLaunchActivity `会调用Instrumentation的` callActivityOnCreate `
```java
mInstrumentation.callActivityOnCreate(activity, r.state, r.persistentState);
```
26 Instrumentation的` callActivityOnCreate `会调用Activity的` performCreate `
```java
activity.performCreate(icicle);
```
27 Activity的` performCreate `会调用自身的onCreate，至此Activity就onCreateonCreate(icicle);
##### 总结
- 和Launcher启动应用程序区别在于应用已启动，在21步的时候，不用新建PID和运行ActivityThread，而是直接启动Activity。
##### 参考
- [Android应用程序内部启动Activity过程（startActivity）的源代码分析](http://blog.csdn.net/luoshengyang/article/details/6703247)
- [任务和返回栈](https://developer.android.com/guide/components/tasks-and-back-stack.html)
