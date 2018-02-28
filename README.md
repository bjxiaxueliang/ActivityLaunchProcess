# Activity启动流程


## 参考
[Activity启动流程](http://blog.csdn.net/qq_23547831/article/details/51224992)
[Android应用启动流程分析](http://solart.cc/2016/08/20/launch_app/)

## Launcher启动App时序图

该时序图来自：[Android应用启动流程分析](http://solart.cc/2016/08/20/launch_app/)

![enter image description here](https://raw.githubusercontent.com/xiaxveliang/ActivityLaunchProcess/master/image/launcher_app.png)

### [点击查看大图](https://raw.githubusercontent.com/xiaxveliang/ActivityLaunchProcess/master/image/launcher_app.png)

## 简化流程


```sequence
participant Launcher进程 as serverA 
participant ActivityManagerService进程 as serverB
participant Zygote进程 as serverC
participant 要启动的APP进程 as serverD
serverA->serverB: startActivity
serverB-->serverA: schedulePauseActivity
serverA-->serverA: ActivityThread中Instrumentation调用onPause
serverA->serverB: 通知AMS Launcher onPaused
serverB->serverC: 创建新进程
serverC->serverD: zygote fork创建新进程
serverD-->serverD: ActivityThread执行main方法
serverD-->serverB: 绑定Application
serverB->serverD: scheduleLaunchActivity
serverD-->serverD: Instrumentation调用onCreate onResume
serverD-->serverB: ActivityResumed
serverB-->serverA: scheduleStopActivity
serverA-->serverA: onStop
```


