
问题：使用ViewPager和Fragment的时候，Fragment不显示内容？

解决办法：


通过大量查询资料发现应该调用getChildFragmentManager(),但是在程序中无法调用getChildFragmentManager()，应该是v4包版本的问题。//后来发现，此推理有误，因为并未使用Fragment的嵌套，所以调用不到getChildFragmentManager()。

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

This is because by using old android.support.v4.

once you change the latest library.it will solved I solved it by change it. Hope it will work

And i think your are trying to use it for android API level less than 17 and the method getChildFragmentManager() is only for API 17 and higher.

[getChildFragmentManager() method is undefined](http://stackoverflow.com/questions/19431839/getchildfragmentmanager-method-is-undefined?rq=1)

**最终解决方案：**

看官方的文档，重新写了一下代码，莫名其妙就又都显示了。。。。这个问题真是莫名其妙。。。。

**代码如下：**
<pre>
<code>
package com.sansi.smarthome.fragment;

import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

import com.sansi.smarthome.R;

/**
 * Created by jack on 2016/4/7.
 */
public class FragmentTwo extends Fragment {
    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        super.onCreateView(inflater, container, savedInstanceState);
        ViewGroup rootView = (ViewGroup) inflater.inflate(R.layout.fragment_two,container,false);

        return rootView;
    }
}

</code>
</pre>

**V1.0.0 结论：**

-  Fragment的使用应该导入V4下的包，嵌套的Fragment需要使用getChildFragmentManager()。
-  Fragment莫名其妙的不显示或者少显示内容真奇怪，必要时重新写一遍MyFragment.calss;

**V1.0.1:**

-  当我重新创建了一个应用，居然才有Viewager +TabLagyout:外面嵌套的是DrawLayout出现所有Fragment都不显示，各种重写修改代码仍不能解决问题，通过log打印信息，发现刚开始会调用Fragment的onCreateView方法，而后就连onCreateView都不调用了。然后我想到是不是因为view的嵌套层次太深，Android手机来不及渲染和绘制，就去掉了嵌套的布局直接：DrawLayout下包含NavigationView和home_content_main，其中home_content_main就是ViewPager和TabLayout。运行程序都显示了。
-  至于网上说的Fragment嵌套不显示数据和我的不是同一个问题，因为我本来就没有嵌套Fragment，这是另一个问题。