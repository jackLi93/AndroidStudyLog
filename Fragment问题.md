
问题：使用ViewPager和Fragment的时候，Fragment不显示内容？

解决办法：


通过大量查询资料发现应该调用getChildFragmentManager(),但是在程序中无法调用getChildFragmentManager()，应该是v4包版本的问题。

-  [Fragment is blank the second time it is viewed](http://stackoverflow.com/questions/7746652/fragment-is-blank-the-second-time-it-is-viewed)
-  [getSupportFragmentManager()和getChildFragmentManager()](http://android.jobbole.com/82416/)

-  [getChildFragmentManager()](http://stackoverflow.com/questions/15805574/android-support-v4-app-fragment-undefined-method-getchildfragmentmanager?rq=1)
-  [Navigation drawer + ViewPager - fragments do not show](http://stackoverflow.com/questions/28691867/navigation-drawer-viewpager-fragments-do-not-show)

经过大量的查询资料和实验我发现：

-  Fragment的xml布局是RelativeLayout的就能显示，比如如下：

	<pre>
	<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="Tab 3"
        android:textAppearance="?android:attr/textAppearanceLarge"/>

<0/RelativeLayout>
	</pre>
但是如果有LinearLayout布局在里面就会有部分内容或者全部内容不显示。真是奇怪！！！


###关于Fragment在ViewPager里不显示View整理一下思路：

一开始：我在项目中使用了TabLayout和ViewPager，ViewPager用来管理Fragment片段。别人都这样使用过，所以逻辑上是没有问题的。

但是，我在项目中发现Fragment的View并不能显示，Log信息显示Fragment的onCreateView有被调用，于是开始查阅资料。

大部分的解决办法是：继承import android.support.v4.app.FragmentStatePagerAdapter这个类，然后
 FragmentManager manager = getChildFragmentManager();按道理来说这样做应该可以解决问题。

但是，我在MainActivity代码中只有这个方法：getSupportFragmentManager()，而没有getChildFragmentManager()，真是奔溃。

于是，继续从头开始捋思路，又遇见神奇的事情，将Fragment的布局用RelativeLayout作为父布局就能显示而用LinearLayout就不能显示。。。。。什么鬼！！！