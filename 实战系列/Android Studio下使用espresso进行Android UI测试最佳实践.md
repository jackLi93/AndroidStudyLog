###Android Studio下使用espresso进行Android UI测试最佳实践

---

**最佳实践**

-  首先，假设我们已经在Android studio下新建了一个Android Project,那么Android studio会自动帮我们生成两个测试包：androidTest和test,**androidTest用于放置UI测试代码，test包下用于放置单元测试代码。**接下来，我们就来一步步创建UI测试代码：

-  第一步：添加依赖
	
	因为我们会使用到相关的测试库，比如我们选择使用espresso进行UI测试，所以首先得在build.gradle文件下添加相关的依赖文件,同时得在defaultconfig中添加<code>testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"</code>。
<pre>

dependencies {
    // App dependencies
    compile 'com.android.support:support-annotations:23.0.1'
    compile 'com.google.guava:guava:18.0'
    // Testing-only dependencies
    // Force usage of support annotations in the test app, since it is internally used by the runner module.
    androidTestCompile 'com.android.support:support-annotations:23.0.1'//001
    androidTestCompile 'com.android.support.test:runner:0.4.1'//002
    androidTestCompile 'com.android.support.test:rules:0.4.1'	//003
    androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.1' //004
}
</pre>

	-  上面的依赖是Google样例的依赖，显然我们可以学习它。
	-  001是注解库，002 runner库，003 为rules库，004为espresso的库。
-  第二步：在project的视角下，找到app-->src-->androidTest--->新建我们的Test文件，同样可以学习Google给的样例代码
-  第三步，使用espresso的语法进行UI测试代码的编写 




---


**遇见的问题**

-  1.在运行UITest的时候，报错，提示信息为：Warning:Conflict with dependency 'com.android.support:support-annotations'

 解决办法：修改依赖的版本，比如将
<pre>androidTestCompile 'com.android.support:support-annotations:23.0.1'</pre>的版本改为提示的版本号。



---
-  2.一定要在build的配置文件中添加 这一行代码：不然会报错提示：junit: no tests found
<code>testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"</code>



-  3.遇见这样的问题：No activities in stage RESUMED. Did you forget to launch the activity.

	解决办法：因为手机屏幕熄灭了，打开手机即可。

---
**Google的测试样例**

-  [Google Test的样例代码](https://github.com/googlesamples/android-testing)

**Google Espresso 教程**

-  [Espresso Advanced Samples](https://google.github.io/android-testing-support-library/docs/espresso/advanced/index.html)

**参考问题**

-  [Espresso - click a single list view item](http://stackoverflow.com/questions/28061300/espresso-click-a-single-list-view-item)

-  [junit: no tests found](http://stackoverflow.com/questions/22469480/junit-no-tests-found)
  
-  [Warning:Conflict with dependency 'com.android.support:support-annotations'](http://stackoverflow.com/questions/32600204/warningconflict-with-dependency-com-android-supportsupport-annotations)
