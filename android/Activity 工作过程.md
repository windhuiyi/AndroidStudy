Activity 工作过程
===============
### Activity 的启动过程
---------------------
#### 应用程序的启动过程
1.Launcher会执行的`startActivity`有几种 [重载方法，]() []() 但最终会调用`startActivityForResult`  
2.Activity 的`startActivityForResult`会调用Instrumentation的`execStartActivity`方法
```Java
Instrumentation.ActivityResult ar = mInstrumentation.execStartActivity(
                     this, mMainThread.getApplicationThread(), mToken, this,
                    intent, requestCode, options);
```
3.Instrumentation的`execStartActivity`方法会调用ActivityManagerNative的`startActivity`
```Java
int result = ActivityManagerNative.getDefault()
                 .startActivity(whoThread, who.getBasePackageName(), intent,
                         intent.resolveTypeIfNeeded(who.getContentResolver()),
                         token, target != null ? target.mEmbeddedID : null,
                         requestCode, 0, null, options);
```

> PS:这个方法最后还会执行`checkStartActivityResult`来检查是否启动成功.  

4.ActivityManagerNative的`gDefault`会通过Binder返回ActivityManagerService的单例。实际上是调用的是ActivityManagerService的`startActivity`方法，会调用`startActivityAsUser` 
``` java
    @Inject
    public OrderListViewModel(OrderRepository repository) {
        super(repository);
    }           
```

5.ActivityManagerService的`startActivityAsUser`会调用ActivityStackSupervisor的`startActivityMayWait`
```java
mStackSupervisor.startActivityMayWait(caller, -1, callingPackage, intent,resolvedType, null, null, resultTo, resultWho, requestCode, startFlags,
    profilerInfo, null, null, options, userId, null, null);  
```
```java
Instrumentation.ActivityResult ar =
                mInstrumentation.execStartActivity(
                    this, mMainThread.getApplicationThread(), mToken, this,
                    intent, requestCode, options);  
```
6 ActivityStackSupervisor的`startActivityMayWait`会调用`startActivityLocked`
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
8 ActivityStackSupervisor的`startActivityUncheckedLocked`会调用ActivityStack的`startActivityLocked`
```java
targetStack.startActivityLocked(r, newTask, doResume, keepCurTransition, options);
```
9 ActivityStack的`startActivityLocked`会ActivityStackSupervisor调用的`resumeTopActivitiesLocked`
```java
mStackSupervisor.resumeTopActivitiesLocked(this, r, options);
```
10 ActivityStackSupervisor的`resumeTopActivitiesLocked`又会调用回ActivityStack的`resumeTopActivityLocked`
```java
 result = targetStack.resumeTopActivityLocked(target, targetOptions);
```
11 ActivityStack的`resumeTopActivityLocked`又会调用自身的`resumeTopActivityInnerLocked`
```java
result = resumeTopActivityInnerLocked(prev, options);
```
12 ActivityStack的`resumeTopActivityInnerLocked`会调用自身的`startPausingLocked`先暂停之前的Activity
```java
pausing |= startPausingLocked(userLeaving, false, true, dontWaitForPause);
```
>PS： 这里会结束掉这个resumeTopActivityInnerLocked，返回true
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
13  ActivityStack的`startPausingLocked`会调用ActivityRecord的ProcessRecord
的IApplicationThread，通过Binder实际上调用ActivityThread的ApplicationThread的`schedulePauseActivity`
```java
prev.app.thread.schedulePauseActivity(prev.appToken, prev.finishing,userLeaving, prev.configChangeFlags, dontWait);  
```
14 ActivityThread的ApplicationThread的`schedulePauseActivity`发送消息最后会调用ActivityThread的`handlePauseActivity`,最后会调用ActivityManagerService的`activityPaused`  
>PS:performPauseActivity会调用Activity的onPause
```java
ActivityManagerNative.getDefault().activityPaused(token);
```
15 ActivityManagerService的`activityPaused`会调用ActivityStack的`activityPausedLocked`
```java
 stack.activityPausedLocked(token, false);
```
16 用ActivityStack的`activityPausedLocked`会调用自身的`completePauseLocked`
```java
completePauseLocked(true);
```
17 用ActivityStack的`completePauseLocked`又会调用ActivityStackSupervisor的`resumeTopActivitiesLocked`
```java
mStackSupervisor.resumeTopActivitiesLocked(topStack, prev, null);
```
18 ActivityStackSupervisor的`resumeTopActivitiesLocked`又会调用回ActivityStack的`resumeTopActivityLocked`
```java
 result = targetStack.resumeTopActivityLocked(target, targetOptions);
```
19 ActivityStack的`resumeTopActivityLocked`又会调用自身的`resumeTopActivityInnerLocked`
```java
result = resumeTopActivityInnerLocked(prev, options);
```
20 ActivityStack的`resumeTopActivityInnerLocked`会调用ActivityStackSupervisor的`startSpecificActivityLocked`
```java
if (next.app != null && next.app.thread != null) {
 ...
}else{
mStackSupervisor.startSpecificActivityLocked(next, true, true);
}
```
21 ActivityStackSupervisor的`startSpecificActivityLocked`,因为应用没有启动，这时会调用ActivityManagerService的`startProcessLocked`
```java
mService.startProcessLocked(r.processName, r.info.applicationInfo, true, 0,
                 "activity", r.intent.getComponent(), false, false, true);
```
22 ActivityManagerService的`startProcessLocked`会调用Process的`start`
```java
Process.ProcessStartResult startResult = Process.start(entryPoint,
                     app.processName, uid, uid, gids, debugFlags, mountExternal,
                     app.info.targetSdkVersion, app.info.seinfo, requiredAbi, instructionSet,
                     app.info.dataDir, entryPointArgs);
```
23 Process的`start`会调用自身的`startViaZygote`
```java
 return startViaZygote(processClass, niceName, uid, gid, gids,
                     debugFlags, mountExternal, targetSdkVersion, seInfo,
                     abi, instructionSet, appDataDir, zygoteArgs);
```
24 Process的`startViaZygote`会调用自身的`zygoteSendArgsAndGetResult`
```java
 return zygoteSendArgsAndGetResult(openZygoteSocketIfNeeded(abi), argsForZygote);
```
>PS:`openZygoteSocketIfNeeded`会打开socket
25 ZygoteInit的`runSelectLoop`会接收到这个消息，然后执行ZygoteConnection的`runOnce`
```java
done = peers.get(index).runOnce();
```
26 ZygoteConnection的`runOnce`会执行Zygote的`forkAndSpecialize`
```java
pid = Zygote.forkAndSpecialize(parsedArgs.uid, parsedArgs.gid, parsedArgs.gids,
                     parsedArgs.debugFlags, rlimits, parsedArgs.mountExternal, parsedArgs.seInfo,
                     parsedArgs.niceName, fdsToClose, parsedArgs.instructionSet,
                     parsedArgs.appDataDir);
```
27 执行完毕后会得到pid，因为pid=0，会调用`handleChildProc`
```java
if (pid == 0) {
                 // in child
                 IoUtils.closeQuietly(serverPipeFd);
                 serverPipeFd = null;
                 handleChildProc(parsedArgs, descriptors, childPipeFd, newStderr);
 
                 // should never get here, the child is expected to either
                 // throw ZygoteInit.MethodAndArgsCaller or exec().
                 return true;
             }
```
28 ZygoteConnection的`handleChildProc`会调用RuntimeInit的`zygoteInit`  
```java
 RuntimeInit.zygoteInit(parsedArgs.targetSdkVersion,parsedArgs.remainingArgs, null /* classLoader */);  
```
29 RuntimeInit的`zygoteInit`会执行`nativeZygoteInit`和`applicationInit`这两个方法
```java
	 nativeZygoteInit();
     applicationInit(targetSdkVersion, argv, classLoader);
```
30 RuntimeInit的`applicationInit`会调用自身的`invokeStaticMain`方法
```java
 invokeStaticMain(args.startClass, args.startArgs, classLoader);
```
31 RuntimeInit的`invokeStaticMain`会抛出`ZygoteInit.MethodAndArgsCaller`这个异常
```java
throw new ZygoteInit.MethodAndArgsCaller(m, argv);
```
30 ZygoteInit的main方法会捕获这个异常，然后调用MethodAndArgsCaller的`run`方法,会执行invoke方法，执行ActivityThread的`main`方法
```java
 mMethod.invoke(null, new Object[] { mArgs });
```
31 ActivityThread的`main`方法会调用自身的`attach`方法
```java
         ActivityThread thread = new ActivityThread();
         thread.attach(false);
```
32 ActivityThread的`attach`方法会调用ActivityManagerService的`attachApplication`
```java
			final IActivityManager mgr = ActivityManagerNative.getDefault();
             try {
                 mgr.attachApplication(mAppThread);
             } catch (RemoteException ex) {
                 // Ignore
             }
```
33 ActivityManagerService的`attachApplication`会调用自身的`attachApplicationLocked`
```java
attachApplicationLocked(thread, callingPid);
```
34 ActivityManagerService的`attachApplicationLocked`会调用ActivityStackSupervisor的`attachApplicationLocked`
```java
 if (mStackSupervisor.attachApplicationLocked(app)) {
                     didSomething = true;
                 }
```
>PS:这个方法不仅启动Activity，还启动Service,还注册Broadcast,完成应用启动的工作。
35 ActivityStackSupervisor的`attachApplicationLocked`会调用自身的`realStartActivityLocked`
```java
if (realStartActivityLocked(hr, app, true, true)) {
                                 didSomething = true;
                             }
```
36 ActivityStackSupervisor的`realStartActivityLocked`会调用ActivityThread的ApplicationThread的`scheduleLaunchActivity`
```java
			app.thread.scheduleLaunchActivity(new Intent(r.intent), r.appToken,
                     System.identityHashCode(r), r.info, new Configuration(mService.mConfiguration),
                     r.compat, r.launchedFromPackage, r.task.voiceInteractor, app.repProcState,
                     r.icicle, r.persistentState, results, newIntents, !andResume,
                     mService.isNextTransitionForward(), profilerInfo);
```
37 ApplicationThread的`scheduleLaunchActivity`最终会调用ActivityThread的`handleLaunchActivity`
38 ActivityThread的`handleLaunchActivity`会调用自身的`performLaunchActivity`
```java
Activity a = performLaunchActivity(r, customIntent);
```
39 ActivityThread的`performLaunchActivity`会调用Instrumentation的`callActivityOnCreate`
```java
mInstrumentation.callActivityOnCreate(activity, r.state, r.persistentState);
```
40 Instrumentation的`callActivityOnCreate`会调用Activity的`performCreate`
```java
activity.performCreate(icicle);
```
41 Activity的`performCreate`会调用自身的onCreate，至此Activity就onCreate.由桌面启动应用程序流程会比较多一点。
onCreate(icicle);
![activity_flow](../../images/activity_flow.png "桌面启动应用流程图")
##### 参考 Android应用程序启动过程源代码分析
[http://blog.csdn.net/luoshengyang/article/details/6689748]



