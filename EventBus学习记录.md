###EventBus学习记录
---

**What is EventBus**

-  Github主页上的介绍是：Android optimized(优化) event bus that simplifies communication between Activities, Fragments, Threads, Services, etc. Less code, better quality
-  一个优化的事件总线，简化Android组件之间的通信，简化代码提高质量。

![EventBus](https://github.com/greenrobot/EventBus/blob/master/EventBus-Publish-Subscribe.png)

-  EventBus is a publish/subscribe event bus optimized for Android.


**Why is EventBus?**

**EventBus...**

-  simplifies the communication between components
	-  decouples event senders and receivers
	-  performs well with Activities, Fragments, and background threads
	-  avoids complex and error-prone dependencies and life cycle issues
-  makes your code simpler
-  is fast
-  is tiny (~50k jar)
-  is proven in practice by apps with 100,000,000+ installs
-  has advanced features like delivery threads, subscriber priorities, etc.

更多参考：

-  [EventBus Features](http://greenrobot.org/eventbus/features/)
-  --

**How To Use EventBus？**

-  EventBus的使用也非常方便，只需要三步走就ok了，当然在使用之前我们得将其添加到依赖库中去：
<pre>
 compile 'org.greenrobot:eventbus:3.0.0'
</pre>
	-  目前最新版本是3.0.0
	
-  **Step 1：定义事件**
-  **Step 2：准备订阅者**
-  **Step 3：发布事件，通知所有的订阅者**
-  这三步在逻辑上也是很清晰的，就好比：1.一个全局广播，2.它的收听者，3.然后广播给所有的收听者。

-  官方教程：[How to get started with EventBus in 3 steps](http://greenrobot.org/eventbus/documentation/how-to-get-started/)

----

**拓展阅读**

-  [EventBus](https://github.com/greenrobot/EventBus):EventBus Github官方主页
-  [Quick Tip: How to Use the EventBus Library
](http://code.tutsplus.com/tutorials/quick-tip-how-to-use-the-eventbus-library--cms-22694)