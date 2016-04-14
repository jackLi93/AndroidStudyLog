####解决了一个Crash:W/ResourceType: Failure getting entry for 0x7f020083
----

**Crash描述：**

-  写的APP在5.0以上可以运行，但是在5.0版本以下的就弹出闪屏页然后Crash掉。



**Crash Log:**
----
04-14 14:08:13.690 11565-11565/com.sansi.smarthome I/dalvikvm: Could not find method android.content.Context.checkSelfPermission, referenced from method com.squareup.leakcanary.DefaultLeakDirectoryProvider.hasStoragePermission
04-14 14:08:13.690 11565-11565/com.sansi.smarthome W/dalvikvm: VFY: unable to resolve virtual method 267: Landroid/content/Context;.checkSelfPermission (Ljava/lang/String;)I
04-14 14:08:13.695 11565-11565/com.sansi.smarthome I/SmartHomeApp: app concreate
04-14 14:08:15.730 11565-11565/com.sansi.smarthome I/Splashactivity: startactivity
04-14 14:08:15.760 11565-11565/com.sansi.smarthome W/dalvikvm: VFY: unable to find class referenced in signature (Landroid/view/SearchEvent;)
04-14 14:08:15.760 11565-11565/com.sansi.smarthome I/dalvikvm: Could not find method android.view.Window$Callback.onSearchRequested, referenced from method android.support.v7.view.WindowCallbackWrapper.onSearchRequested
04-14 14:08:15.760 11565-11565/com.sansi.smarthome W/dalvikvm: VFY: unable to resolve interface method 18914: Landroid/view/Window$Callback;.onSearchRequested (Landroid/view/SearchEvent;)Z
04-14 14:08:15.765 11565-11565/com.sansi.smarthome I/dalvikvm: Could not find method android.view.Window$Callback.onWindowStartingActionMode, referenced from method android.support.v7.view.WindowCallbackWrapper.onWindowStartingActionMode
04-14 14:08:15.765 11565-11565/com.sansi.smarthome W/dalvikvm: VFY: unable to resolve interface method 18918: Landroid/view/Window$Callback;.onWindowStartingActionMode (Landroid/view/ActionMode$Callback;I)Landroid/view/ActionMode;
04-14 14:08:15.880 11565-11565/com.sansi.smarthome I/MainActivity.class: onCreate
04-14 14:08:15.880 11565-11565/com.sansi.smarthome I/MainActivity.class: 192内存大小
04-14 14:08:15.885 11565-11565/com.sansi.smarthome I/dalvikvm: Could not find method android.content.res.TypedArray.getChangingConfigurations, referenced from method android.support.v7.widget.TintTypedArray.getChangingConfigurations
04-14 14:08:15.885 11565-11565/com.sansi.smarthome W/dalvikvm: VFY: unable to resolve virtual method 444: Landroid/content/res/TypedArray;.getChangingConfigurations ()I
04-14 14:08:15.885 11565-11565/com.sansi.smarthome I/dalvikvm: Could not find method android.content.res.TypedArray.getType, referenced from method android.support.v7.widget.TintTypedArray.getType
04-14 14:08:15.885 11565-11565/com.sansi.smarthome W/dalvikvm: VFY: unable to resolve virtual method 466: Landroid/content/res/TypedArray;.getType (I)I


**重要的信息：**

 -  04-14 14:08:15.925 11565-11565/com.sansi.smarthome W/ResourceType: Failure getting entry for 0x7f020083 (t=1 e=131) in package 0 (error -75)
-  04-14 14:08:15.925 11565-11565/com.sansi.smarthome W/dalvikvm: threadid=1: thread exiting with uncaught exception (group=0x41935c50)

-  04-14 14:08:17.400 11565-11565/com.sansi.smarthome I/Process: Sending signal. PID: 11565 SIG: 9

**Crash分析**

-  一开始我以为是低版本的手机内存不够用，但是打印出内存信息192M,比我红米上的128M还要大，所以显然不是这个原因。
- 然后看见  threadid=1: thread exiting with uncaught exception这一行信息，感觉像是哪里的线程不对，但是经过分析和实验得以排除。
- 再分析这段代码信息：W/ResourceType: Failure getting entry for 0x7f020083 (t=1 e=131) in package 0 (error -75)
	- 通过查询，发现是不能获取某一个资源，然后去寻找0x7f020083 代表的意思，在stacksoverflow上发现这是R.java文件下的资源代号，即每一个资源都有一个int代号作为索引。
	- 然后去寻找androidstudio文件下的R文件位置，在project视角下-->app-->build-->...
	- 发现0x7f020083对应的是一个图片资源public static final int iconfont_dot1=0x7f020083 。
	- 然后我去drawable/文件下去查看，发现了一个惊天的秘密，就是程序在5.0以下crash的根本原因：iconfont_dot1（v21）,这个图片资源代表v21以上才能使用的，即5.0版本以上的图片资源。于是问题解决了：找到所有v21的图片资源，再复制添加到普通的drawable/文件资源。
	- **注意：在我们往drawable文件下复制图片的时候，android Studio会让我们选择drawable21还drawable版本，要想所有版本的android都能使用到该图片资源当然选择复制到drawable/下。**

**小结**

-  下次再遇见**W/ResourceType: Failure getting entry for 0x7f020083**这样的Crash，就应该知道是有些资源访问受限。

**参考文档：**

-  [Android Studio 下R文件的位置](http://bbs.csdn.net/topics/391052139)
-  [W/ResourceType(463): Failure getting entry in package 0 (error -75) in Android Activity](http://resourcetype.android.codesolution.site/478385-9171-resourcetype-463-failure-getting-entry-package-error-android-activity.html)
