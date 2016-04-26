###ViewPager中Fragment与宿主Activity通信的问题002

**场景再现**

项目中的MainActivity采用的是**TabLayout+ViewPager**的方式，ViewPager中的Fragment中有扫一扫添加的功能，那么问题来了，我扫一扫获得的结果该如何处理？？类似情形如：微信扫一扫添加好友。

**解决办法**

在MainActivity的onActivityResult对数据进行处理：
<pre>
 @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        IntentResult result = IntentIntegrator.parseActivityResult(requestCode, resultCode, data);
        if(result != null) {
            if(result.getContents() == null) {
                Log.d("MainActivity", "Cancelled scan");
                Toast.makeText(this, TAG+"Cancelled", Toast.LENGTH_LONG).show();
            } else {
                Log.d("MainActivity", "Scanned");
                Toast.makeText(this, TAG+"Scanned: " + result.getContents(), Toast.LENGTH_LONG).show();
            }
        } else {
            // This is important, otherwise the result will not be passed to the fragment
            super.onActivityResult(requestCode, resultCode, data);
        }
    }
</pre>
这样一写确实能够将扫描到的数据Toast。但是同时产生了一个疑问：Fragment 和Activity都有onActivityResult的方法，二者有什么区别呢？

-  我做了如下的实验：
	-  1.只重写MainActivity的onActivityResult方法，可以toast出数据
	-  2.只重写Fragment的onActivityResult的方法，不能toast数据
	-  3.同时重写MainActivity和Fragment的onActivityResult方法，能toast数据
	
-  通过实验看上去，Fragment的onActivityResult方法好像没有什么用？但是二者到底是什么关系呢？


**Activity 和Fragment的onActivityResult二者的关系**

-  来自stack上的讨论：[Fragment onActivityResult method on executing calls activity onActivityResult](http://stackoverflow.com/questions/16434617/fragment-onactivityresult-method-on-executing-calls-activity-onactivityresult)
-  大致意思是：首先MainActivity的onActivityResult会进行拦截，然后再会根据代码的条件传递给下面的FragmentonActivityResult调用。

**相关阅读**

-  [Android的Fragment中onActivityResult不被调用的终极解决方案](http://jameszhao84.iteye.com/blog/2208433)
-  