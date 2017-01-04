---
title: Android闹钟AlarmManager
date: 2017-01-03 17:30:50
categories: Android
tags:
	- Android
---
![](/images/start1.jpg)  
# Android 闹钟开发设置 #
---
> 本文主要是为了自己做闹钟开发过程中学到的知识点，以及踩到的坑。Android的闹钟和IOS 的闹钟不一样，IOS 的闹钟有设置个数上限50个，Android我查阅了相关资料，并没有说到有设置闹钟上限的说法，如果哪位朋友有查到相关的信息，欢迎交流。众所周知，Android因为是开源，所以存在各种各样的版本，因此开发过程中会遇到在这个手机上能实现的功能，在另一个手机就不能运行的情况，我在做闹钟的情况也遇到过这种情况。下文会把自己学到的知识点，以及踩的坑都会记录下来。

# Android 闹钟功能的实现 #
---
## AlarmManager 的使用 ##
> Anroid 闹钟功能的实现，是系统底层已经帮忙实现好了的，并且提供了接口供我们使用，封装在AlarmManager类中。我们当前先分析介绍下AlarmManager 提供的接口。

### 设置闹钟 ###

![](/images/Android闹钟AlarmManager/alarmanager_interface.png)

图中紫色箭头所指向的三个方法，便是系统提供的设置闹钟的方法。
` public void set(int type, long triggerAtMillis, PendingIntent operation) ` 是设置一次闹钟提醒。  
` public void setRepeating(int type, long triggerAtMillis,long intervalMillis, PendingIntent operation) `是设置重复提醒的闹钟。  
` public void setWindow(int type, long windowStartMillis, long windowLengthMillis, PendingIntent operation)  `是API 19之后设置一次闹钟。  
 注意：Android闹钟机制在API 19之后，为了节约电量和电池使用，将闹钟由准确传递都变成非准确传递。如下图： 
 
![](/images/Android闹钟AlarmManager/api19.png)  

所以在Android开发闹钟的时候，要区分APi版本使用设置闹钟的接口。  

	
	//        PendingIntent sender = PendingIntent.getBroadcast(context, id, intent, PendingIntent
	//                .FLAG_CANCEL_CURRENT);
	PendingIntent sender = PendingIntent.getService(context, id, intent, PendingIntent
		                .FLAG_UPDATE_CURRENT);
	  if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
	            am.setWindow(AlarmManager.RTC_WAKEUP, calMethod(week, calendar.getTimeInMillis()),
	                    intervalMillis, sender);
	        } else {
	            if (flag == 0) {// flag 等于0，设置一次闹钟，flag 不等于0，设置重复闹钟。
	                am.set(AlarmManager.RTC_WAKEUP, calendar.getTimeInMillis(), sender);
	            } else {
	                am.setRepeating(AlarmManager.RTC_WAKEUP, calMethod(week, calendar.getTimeInMillis
	                        ()), intervalMillis, sender);
	            }
	        }  
### 取消闹钟 ###

Android 取消闹钟的接口就只有一个。如下图：  

![](/images/Android闹钟AlarmManager/cancel.png)

Android中取消闹钟，只有一个cancel 接口：  

` public void cancel(PendingIntent operation) `  

注意：  
> 1. Android中要想取消闹钟，因为Android只有一个cancel 接口，并且只传入一个PendingIntent类型的参数，所以要想准确的取消闹钟，必须传入的PendingIntent要和设置闹钟时候传入的PendingIntent一致，否则无法取消闹钟,注意，设置的class 和 action 要和设置闹钟class 和action保持一致；  
> 2. 要想取消闹钟，PendingIntent初始化中第四个参数必须是` PendingIntent.FLAG_UPDATE_CURRENT `.如下图紫色箭头所指：  

![](/images/Android闹钟AlarmManager/pendingIntent.png)

# Android 闹钟进程保活 #
---

> Android 闹钟响应是通过PendingIntent实现的，设置闹钟的时候会传入一个PendingIntent，同时传入一个距离目前时间的间隔值，到了时间之后，系统便会发出一个消息，然后由PendingIntent去接收消息，并且处理消息。PendingIntent的初始化方式有三种，也就是说，Android闹钟的响应方式有三种，如下图：   

![](/images/Android闹钟AlarmManager/pendingIntent_init.png)

如图中所示，获取PendingIntent有三种方式，可以通过Activty，Broadcast,Service 三种方式去获取PendingIntent，响应闹钟发回的消息。  
Android 常用的闹钟消息处理机制是使用的broadcast和Service，Android 中消息是通过intent来传递的，broadcast 消息的本质也是intent，自从Android 3.1之后，所有的intent 消息，系统会自动给它加上` FLAG_EXCLUDE_STOPPED_PACKAGES ` ,这就导致了，如果app处于停止状态，也就是进程被杀死之后，就无法接收到广播的消息了。想要进程被杀死，也就是停止状态的app 也能够接收到消息，那么需要给消息的intent添加 ` FLAG_INCLUDE_STOPPED_PACKAGES ` ,但是由于闹钟的消息是由系统发出来的，我们无法去修改它，所以，我们无法修改Intent，所以我们就尝试去让程序进程保活。

## MarsDaemon进程保活使用 ##
MarsDaemon的github地址：   

[https://github.com/Marswin/MarsDaemon](https://github.com/Marswin/MarsDaemon)


### 第一步 ###
1. 确定自己需要常驻进程的服务，创建一个和它处在同一进程的receiver ,然后新开一个进程创建一个service和receiver,注意：都在在AndroidManifest中注册，进程名可以自己定义：如下图：  

![](/images/Android闹钟AlarmManager/service.png)  

service1是有业务处理的需要常驻进程的Service，其他三个组件都是额外创建的，里面不需要做任何事情，空实现就可以。

### 第二步 ###
2. 用你的Application继承DaemonApplication，然后在回调方法getDaemonConfigurations中返回一个配置，将刚才注册的进程名，service类名，receiver类名传进来。如下图：  

![](/images/Android闹钟AlarmManager/damo.png)  

注意图中紫色箭头所指的地方，必须是进场所在的包名和类名。
### 第三步 ###
杀掉进程尝试。   
[感谢猫九爷阳果果博客参考](http://blog.csdn.net/marswin89/article/details/50917098)