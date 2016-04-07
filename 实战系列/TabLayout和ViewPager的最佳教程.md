###实战之最佳教程：TabLayout和ViewPager的最佳教程

---

开发TabLayout请看这篇教程：//需翻墙

-  [Android Material Design working with Tabs](http://www.androidhive.info/2015/09/android-material-design-working-with-tabs/)

-[Android Tabs Example – With Fragments and ViewPager](http://www.truiton.com/2015/06/android-tabs-example-fragments-viewpager/)

---

**TabLayou与ViewPager的核心代码是：**
<pre>
<code>
       ViewPager  mViewPager = (ViewPager) findViewById(R.id.viewPager);
       setupViewPager(mViewPager);//自定义的方法，为ViewPager设置Adapter
       TabLayout mTabLayout = (TabLayout) findViewById(R.id.tabLayout);
       mTabLayout.setupWithViewPager(mViewPager);//连接Tab与ViewPager的方法
       setupTabIcons();//自定义方法，为Tabs设置Icon
</code>
</pre>
代码逻辑：

 -  找到xml布局中的ViewPager对象，设置ViewPager的适配器，适配器是自定义的继承自FragmentPagerAdapter。
 -  找到布局文件中的TabLayout对象，mTabLayout.setupWithViewPager(mViewPager);这行代码特别重要因为它将ViewPager于TabLayout连接到了一起。如果不加此方法程序会出现异常。
 -  setupTabIcons();是自定义的方法为Tab设置Icon。

**注意的地方：**

-  在导入Fragment包的时候注意选择与ViewPager像匹配的包：
<pre>
import android.support.v4.app.Fragment;
</pre>
- 在使用ViewPager与fragment的时候，Fragment视图不显示。解决办法目前尚不明确具体稳定的解决办法，只知道将布局换成RelativeLayou就可以显示，换成LinearLayout布局就会导致不显示。 

**拓展阅读：**

-  来自简书：[TabLayout+ViewPager 简单实现app底部Tab布局](http://www.jianshu.com/p/adf7a994613a)
-  [Android底部导航总结](http://www.androidchina.net/1263.html):ViewPager会自动家族下一页的Fragment，这篇文章有给解决办法。