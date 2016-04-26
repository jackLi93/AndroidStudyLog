###使用ViewPager自动轮播引起的内存泄漏

**问题描述**

-  MainActivity中是TabLayout+ViewPager的组合，然后在第一个Tab的Fragment中又使用了ViewPager来自动轮播图片，当我点击按钮来回跳转MainActivity和其他Activity的时候，发现内存泄漏直至OOM，APP Crash掉，但是关掉自动轮播之后，来回跳转Activity的时候，并未出现内存泄漏，程序运行良好。

**问题分析**

-  首先，我们对图片进行了优化，然后根据控制变量法来探究问题，我们可以确定问题就出在**自动轮播**上。
-  自动轮播开启了新的线程，然后新线程所引用的对象在Activity结束后没有及时释放所持有的对象，比如图片资源。

**然后看看StackOverFlow是如何实现ViewPager自动轮播的？**

-  [Android ViewPager automatically change page](http://stackoverflow.com/questions/17167740/android-viewpager-automatically-change-page)

**解决问题**
	
-  首先，这是关于内存泄漏的问题，更多产生内存泄漏的原因参考拓展阅读。
-  然后，基于此场景就是需要在结束的时候释放资源：

<pre>
 //所以我们在应用退出时，要将线程销毁，我们只要在Activity中的，onDestory()方法处理一下就OK了，如下代码所示:
 @Override  
   protected void onDestroy() {  
     mHandler.removeCallbacks(mRunnable);  //这一行很重要
     super.onDestroy();  
   } 
  
</pre>

**最终，此问题得以解决，完整代码如下：**

-  第一步：定义Handler和 Runnable的对象
<pre>
 private Handler mHandler= new Handler();
    public static final int DELAY = 2000;

    Runnable mRunnable = new Runnable()
    {

        @Override
        public void run()
        {
            int position=advertisementViewPager.getCurrentItem();
            position++;
            if(position>Integer.MAX_VALUE ){
                position=0;
            }
            advertisementViewPager.setCurrentItem(position);
            mHandler.postDelayed(mRunnable, DELAY);
        }
    };
</pre>

-  第二步：在OnCreate方法中进行调用，开启自动轮播：
<pre>
mHandler.postDelayed(mRunnable, DELAY);
</pre>

-  第三步：经过以上两步已经能够实现自动轮播，但是没有第三步的话肯定有内存泄漏

<pre>
    @Override
    public void onDestroy() {
        super.onDestroy();
        mHandler.removeCallbacks( mRunnable );
    }
</pre>


**展开阅读**

-  [android viewpager out of memory](http://stackoverflow.com/questions/18144132/android-viewpager-out-of-memory)
-  [关于Android 的内存泄露及分析](http://www.cnblogs.com/tiantianbyconan/p/3679875.html)