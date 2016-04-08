需要解决问题：单个帖子进入时候可以实现左右切换，并且是在不知道帖子总数的情况下，就是不知道ViewPagerAdapter的getCount数量。

由于帖子内容的数据图片和布局比较复杂，所以不让ViewPager缓存，否则消耗内存太大

**不要让ViewPager缓存**

**不要让ViewPager缓存**

**不要让ViewPager缓存**

 因为我在项目中，应用了ViewPager加Fragment,产生了OOM,内存溢出，从头到尾分析了好久，才发现是因为ViewPager预加载的原因。
[StackOverFlow上因为预加载而OOM](http://stackoverflow.com/questions/19096868/how-can-make-my-viewpager-load-only-one-page-at-a-time-ie-setoffscreenpagelimit)

**解决方案：**

-  设置ViewPager的缓存界面数：
	-  mPager .setOffscreenPageLimit(2);
	-  参数：int limit  
	  -    缓存当前界面每一侧的界面数

	-  以上述为例，当前界面为1，limit = 2，表示缓存2、3两个界面。如此便避免了界面3被销毁。

**参考文档：**

-  [ViewPager中切换界面Fragment被销毁的问题分析](http://www.cnblogs.com/monodin/p/3866441.html)


-  [ViewPager默认预加载为1](http://www.eoeandroid.com/thread-155965-1-1.html)

-  [Fragment的setUserVisibleHint方法实现懒加载](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/1021/1813.html)