##RxJava,RxAndroid与Retrofit的初次谋面

**学习目标：**
掌握Rxjava 和RxAndroid的基本概念，熟悉RxJava或者RxAndroid与Retrofit的配合使用。

**开始时间：**2016-4-5

**学习方式：**
阅读博客，Google加关键字搜索资料

**学习契机：**
	在github中关注的Android 开发前线的项目中，阅读到一篇翻译的文章：《利用Retrofit和RxJava实现服务器轮询和出错重试》，展开了对RxJava，RxAndroid Retrofit配合使用的学习。

---
####关于RxJava

**什么是RxJava：**

-  在了解**RxJava**之前需要先了解**ReactiveX**，因为RxJava是对ReactiveX基于Java语言的实现，所以ReactiveX的编程思想才是RxJava，RxAndroid等Rx系列的最顶层的思想。

---
**那么什么是ReactiveX?**

-  [ReactiveX.io]((http://reactivex.io/))给的定义是，Rx是一个使用可观察数据流进行**异步编程**的编程接口，**ReactiveX结合了观察者模式、迭代器模式和函数式编程的精华。**
-  ReactiveX is a library for composing asynchronous and event-based programs by using observable sequences.
-  ReactiveX is a combination of the best ideas from
**the Observer pattern**, **the Iterator pattern**, and **functional programming**。
-  **ReactiveX宣言:** ReactiveX不仅仅是一个编程接口，它是一种编程思想的突破，它影响了许多其它的程序库和框架以及编程语言。
  -  抓住关键词：**异步编程**，**拓展观察者模式**，**编程思想**。
   
### **Rx模式核心思想**


**使用观察者模式**

-  创建：Rx可以方便的创建事件流和数据流
-  组合：Rx使用查询式的操作符组合和变换数据流
-  监听：Rx可以订阅任何可观察的数据流并执行操作

**简化代码**

-  函数式风格：对可观察数据流使用无副作用的输入输出函数，避免了程序里错综复杂的状态
-  简化代码：Rx的操作符通通常可以将复杂的难题简化为很少的几行代码
-  异步错误处理：传统的try/catch没办法处理异步计算，Rx提供了合适的错误处理机制
-  轻松使用并发：Rx的Observables和Schedulers让开发者可以摆脱底层的线程同步和各种并发问题

关于更多ReactiveX的知识，请参考ReactiveX中文档和官方主页。

----

**现在再看看什么是RxJava，就容易理解多了：**

-  RxJava is a Java VM implementation of Reactive Extensions: a library for composing asynchronous and event-based programs by using observable sequences.
-  RxJava是对Reactive编程思想的基于Java语言的实现，继承了Rx的异步编程，观察者模式等特性。	

---
**工作来了，未完待续。。。。**

---
**参考文档：**

-  [给 Android 开发者的 RxJava 详解](http://gank.io/post/560e15be2dca930e00da1083)
-  [RxAndroid初探](http://coderrobin.com/2015/07/17/RxAndroid%E5%88%9D%E6%8E%A2/)
-  [利用Retrofit和RxJava实现服务器轮询和出错重试](https://github.com/bboyfeiyu/android-tech-frontier/blob/master/issue-44/%E5%88%A9%E7%94%A8Retrofit%E5%92%8CRxJava%E5%AE%9E%E7%8E%B0%E6%9C%8D%E5%8A%A1%E5%99%A8%E8%BD%AE%E8%AF%A2%E5%92%8C%E5%87%BA%E9%94%99%E9%87%8D%E8%AF%95.md)
-  [RxJava教程集](https://github.com/bboyfeiyu/android-tech-frontier/tree/master/rxjava)

**Github:**

-  [RxAndroid项目主页](https://github.com/ReactiveX/RxAndroid)
-  [ReactiveX项目主页](https://github.com/ReactiveX)

**网站主页：**

-  [ReactiveX网站主页](http://reactivex.io/)
-  [ReactiveX中文翻译](https://mcxiaoke.gitbooks.io/rxdocs/content/Intro.html)