##Android View体系

**What is View ?**
	
-  view 是Android中可见的一块矩形区域，有自己的宽度和高度
-  View 是Android中所有控件的基类，我们所有可见的控件都继承自View
-  view是一种界面层的控件的抽象，它有一套自己的绘制流程：--->布局--->测量--->绘制
-  view能捕获和响应用户的点击事件
-  google 给出的View的定义:
	-  Provides classes that expose basic user interface classes that handle screen layout and interaction with the user
	-  **解释为：**能够处理界面布局和用户交互的基础控件

**Android里的View和ViewGroup体系结构视图**


![view 体系](http://ww2.sinaimg.cn/mw690/87dacc16gw1ez2rbxd8lsj20mw0d9wfy.jpg)

**关于界面的绘制**

-  我们知道Android中的界面是各种ViewGroup和View的嵌套组合而成的，它们构成了一个树的层次结构，Android系统在绘制界面的时候就是从根布局到子布局一层一层的渲染出来的，但是根据Google给的关于优化性能的教程中，我们知道1s中60帧可以在手机屏幕上为人眼所接受，所以每一个界面的绘制时间为16ms,也就是说当你的布局嵌套层次过多的时候，如果在16ms内Android系统无法完成所有子布局的渲染绘制，就会导致布局的丢失。
-  另外，Google开发者文档告诉我们：在布局中RelativeLayout比LinearLayout所需要渲染的时间要少，所以在开发中可以优先考虑使用RelativeLayout。

**展开阅读**

-  [Android性能优化典范 - 第1季](http://hukai.me/android-performance-patterns/)：对Android如何绘制界面有形象深刻的介绍。
-  [Improving Layout Performance](http://www.jianshu.com/p/2a7d6b20cc3f)
-  [Android界面性能调优手册](https://androidtest.org/android-graphics-performance-pattens/)

---

**Android UI界面架构**

 
![UI界面架构](http://hujiaweibujidao.github.io/images/androidheros_ui.png)

-  每个Activity包含一个PhoneWindow对象，PhoneWindow设置DecorView为应用窗口的根视图。在里面就是熟悉的TitleView和ContentView,没错，平时使用的setContentView()就是设置的ContentView。


**展开阅读：**

-  [Android View总结](http://threezj.com/2015/12/17/Android%20View%E8%AF%A6%E8%A7%A3/)
-  [Google开发者文档之View](http://developer.android.com/intl/zh-cn/reference/android/view/View.html)
