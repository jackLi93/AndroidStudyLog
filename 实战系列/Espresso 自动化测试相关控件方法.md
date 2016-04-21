### Espresso 自动化测试相关控件方法

---
**参考文章**

-  这是cadn博客上的系列自动化测试文章：[Espresso 自动化测试](http://blog.csdn.net/qq744746842/article/category/6088564)
-  这篇文章介绍了除了基本的Espresso之外，如何测试RecyclerView等控件的依赖库：[UI Testing with Espresso](https://guides.codepath.com/android/UI-Testing-with-Espresso#interacting-with-a-listview)

-  包含了一些特殊控件的测试类：[android.support.test.espresso.contrib](http://developer.android.com/intl/zh-cn/reference/android/support/test/espresso/contrib/package-summary.html)


**在测试DraweLayout遇见的问题**

-  [openDrawer from espresso contrib is deprectaded](http://stackoverflow.com/questions/32654554/opendrawer-from-espresso-contrib-is-deprectaded)

**注意：**

1. 添加依赖包需要这样添加：否则程序会crash，出现空指针异常
<pre>
<code>
    androidTestCompile ('com.android.support.test.espresso:espresso-contrib:2.2.1'){
        exclude group: 'com.android.support', module: 'appcompat'
        exclude group: 'com.android.support', module: 'support-v4'
        exclude module: 'recyclerview-v7'
    }
</code>
</pre>

 2.导入2.2.1'及以后的版本，因为才2.2.1'及之后的版本才有DrawerActions.open()和DrawerActions.close()方法。


**测试侧滑菜单**

-  [Android - How to click on an item on a navigation drawer using Espresso?](http://stackoverflow.com/questions/35944723/android-how-to-click-on-an-item-on-a-navigation-drawer-using-espresso?rq=1)

**测试TabLayout**

-  [SlidingTabLayout.java](https://android.googlesource.com/platform/frameworks/testing/+/android-support-test/espresso/sample/src/main/java/android/support/test/testapp/SlidingTabLayout.java)

**测试自定义seekBar**

- [Espresso - Set SeekBar](http://stackoverflow.com/questions/23659367/espresso-set-seekbar)

**测试TabLayout的思路**

-  [How open tab in Espresso](http://stackoverflow.com/questions/25016397/how-open-tab-in-espresso)
参考了上面的思路，我在进行tabLayout的点击UI测试的时候，使用了如下代码，并能完成自动UI测试。但是有一点不足的地方：就是原来我使用的是R.string.home_tab espressoUI测试就报错提示：寻找到不止一个房间字符，所以我重新创建了R.string.home_tab_test等资源，(R.string.home_tab_test=房间00),如此能完成自动测试，但是几个tab就变成了首页00，房间00，模式00，设置00，而原意为首页，房间，模式，设置。。。
<pre>
<code>
        onView(withText(R.string.home_tab_test)).perform(click());
        onView(withText(R.string.room_tab_test)).perform(click());
        onView(withText(R.string.model_tab_test)).perform(click());
        onView(withText(R.string.setting_tab_test)).perform(click());
        onView(withText(R.string.home_tab_test)).perform(click());
</code>
</pre>
-  [android项目中values中ids.xml的作用](http://blog.csdn.net/songguobing/article/details/9308127)