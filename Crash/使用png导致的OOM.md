##内存溢出

1.  问题：在使用新的库进行App开发的时候，使用了几个png的icon，运行程序，App就Crash，提示信息是：OOM。
	
**分析问题：**
	
通过对比Android Studio中自带的drawable/下的文件，我发现它都是使用xml文件来表示的图片资源，代码如下:

	<vector xmlns:android="http://schemas.android.com/apk/res/android"
    android:width="24dp"
    android:height="24dp"
    android:viewportHeight="24.0"
    android:viewportWidth="24.0">
    <path
        android:fillColor="#FF000000"
        android:pathData="M12,12m-3.2,0a3.2,3.2 0,1 1,6.4 0a3.2,3.2 0,1 1,-6.4 0" />
    <path
        android:fillColor="#FF000000"
        android:pathData="M9,2L7.17,4H4c-1.1,0 -2,0.9 -2,2v12c0,1.1 0.9,2 2,2h16c1.1,0 2,-0.9 2,-2V6c0,-1.1 -0.9,-2 -2,-2h-3.17L15,2H9zm3,15c-2.76,0 -5,-2.24 -5,-5s2.24,-5 5,-5 5,2.24 5,5 -2.24,5 -5,5z" />
	</vector>
通过查询资料，发现这是一种新的图片表示格式。在Android Support Library 23.2被引入。

-  这个工具可以将SVG格式图片转为xml文件。[Android SVG to VectorDrawable](http://inloop.github.io/svg2android/)
-  参考文章：[近乎通用的VectorDrawable](https://github.com/hehonghuidev/android-tech-frontier/blob/master/issue-35/%E8%BF%91%E4%B9%8E%E9%80%9A%E7%94%A8%E7%9A%84VectorDrawable.md)


**网上答案**

-  [I switched to 23.2.0 or 23.2.1 my app suffers with OOM ](https://code.google.com/p/android/issues/detail?id=205236)

**相关问题：**

- [mipmap和drawable文件夹的区别](http://stackoverflow.com/questions/28065267/mipmap-vs-drawable-folders)

-  [Android内存优化之OOM](http://hukai.me/android-performance-oom/)

-  [Android应用中OOM问题剖析和解决方案](http://www.runoob.com/w3cnote/android-oom.html)

- [Android开发中应该避免的内存泄露](http://mtc.baidu.com/academy/detail/article/9)

-  [Android内存泄漏定位与解决](http://mtc.baidu.com/academy/detail/article/12)

-  [每个Android开发者必须知道的内存管理知识](http://www.codeceo.com/article/android-memory-manage.html)