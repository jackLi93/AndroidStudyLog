###源码笔记：Splash界面滑动导航+各种切换动画自动轮播效果

**What**

-  这是一篇关于Splash界面滑动导航+各种切换动画自动轮播效果的读码笔记
-  [BGABanner-Android](https://github.com/jackLi93/BGABanner-Android)：这个实现了广告轮播效果的Lib只有三个Class文件：
	-  BGAViewPgaer：继承了ViewPager的类，通过反射方式实现支持低版本上切换动画，以及是否允许左右滑动
	-  BGABanner:继承了RelativeLayout的类，是一个完整的自定义View的业务类，封装了BGAViewPager等，实现了广告栏的所有业务
	-  PageChangeDurationScroller：继承了Scroller，可以传入mDuration，即滑动时间
  

**效果图：**
![引导页](https://raw.githubusercontent.com/bingoogolapple/BGABanner-Android/server/screenshots/banner1.gif)
![广告栏动画](https://raw.githubusercontent.com/bingoogolapple/BGABanner-Android/server/screenshots/banner2.gif)

**自定义ViewPager中学到的套路：**

-  继承ViewPager，重写setPageTransformer方法，移除版本限制，通过反射设置参数和方法
-  在重写setPageTransformer的过程中，学习了对反射的应用
-  代码如下：

<pre>
@Override
    public void setPageTransformer(boolean reverseDrawingOrder, ViewPager.PageTransformer transformer) {
        /**
         继承ViewPager，重写setPageTransformer方法，移除版本限制，通过反射设置参数和方法

         getDeclaredMethod*()获取的是【类自身】声明的所有方法，包含public、protected和private方法。
         getMethod*()获取的是类的所有共有方法，这就包括自身的所有【public方法】，和从基类继承的、从接口实现的所有【public方法】。

         getDeclaredField获取的是【类自身】声明的所有字段，包含public、protected和private字段。
         getField获取的是类的所有共有字段，这就包括自身的所有【public字段】，和从基类继承的、从接口实现的所有【public字段】。
         */
        Class viewpagerClass = ViewPager.class;

        try {
            boolean hasTransformer = transformer != null;

            Field pageTransformerField = viewpagerClass.getDeclaredField("mPageTransformer");//获取属性
            pageTransformerField.setAccessible(true);//设置可接入
            PageTransformer mPageTransformer = (PageTransformer) pageTransformerField.get(this);//获取该属性对象

            boolean needsPopulate = hasTransformer != (mPageTransformer != null);
            pageTransformerField.set(this, transformer);//设置动画

            Method setChildrenDrawingOrderEnabledCompatMethod = viewpagerClass.getDeclaredMethod("setChildrenDrawingOrderEnabledCompat", boolean.class);
            setChildrenDrawingOrderEnabledCompatMethod.setAccessible(true);
            setChildrenDrawingOrderEnabledCompatMethod.invoke(this, hasTransformer);

            Field drawingOrderField = viewpagerClass.getDeclaredField("mDrawingOrder");
            drawingOrderField.setAccessible(true);
            if (hasTransformer) {
                drawingOrderField.setInt(this, reverseDrawingOrder ? 2 : 1);
            } else {
                drawingOrderField.setInt(this, 0);
            }

            if (needsPopulate) {
                Method populateMethod = viewpagerClass.getDeclaredMethod("populate");
                populateMethod.setAccessible(true);
                populateMethod.invoke(this);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
</pre>
	
-  代码分析：
	-  阅读代码发现这里的反射用的挺有**套路**的：获取Class对象-->调用方法获取其的相关属性和相关方法--->然后对其相关属性和方法进行修改 --->再调用修改后的属性或者方法--->实现了我们想要的功能。
**拓展阅读：**
-  [反射（Reflection）](https://github.com/JustinSDK/JavaSE6Tutorial/blob/master/docs/CH16.md)

----
	
**自定义View时常会用到的一个类：**

- [TypedArray语法详解](http://www.cnblogs.com/LiesSu/p/3862319.html):用于管理自定义属性

---
####演示demo中：
**使用Retrofit库获取服务器图片**
	
-  这样就可以愉快的打广告了
-  关于Retrofit网上也有很多资料，我自己也对它进行了学习总结

**使用了图片缓存库Glide**

-  github地址：[glide](https://github.com/bumptech/glide)

相关阅读：

-  [Google推荐的图片加载库Glide介绍](http://jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0327/2650.html)
-  [Glide 一个专注于平滑滚动的图片加载和缓存库](http://www.jianshu.com/p/4a3177b57949)

一行代码：
 
<pre>Glide.with(MainActivity.this).load(bannerModel.imgs.get(i)).placeholder(R.drawable.holder).error(R.drawable.holder).into(imageView);</pre>
**展开阅读：**

-  [Google推荐的图片加载库Glide介绍](http://jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0327/2650.html)
-  [Glide 一个专注于平滑滚动的图片加载和缓存库](http://www.jianshu.com/p/4a3177b57949)
-  [TypedArray语法详解](http://www.cnblogs.com/LiesSu/p/3862319.html)