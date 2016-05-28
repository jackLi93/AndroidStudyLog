##Android 屏幕适配

-  android手机厂商众多，屏幕碎片化严重，有大屏手机，还有小屏手机，有高清还有普清手机，一款好的Android APP必须解决屏幕适配的问题。

**解决办法**

-  首先，明确下解决屏幕适配的相关思路：
	-  最简单粗暴的方法，为不同的屏幕创造不同的布局，缺点：要做很多套，费时费力
	-  可以采用灵活的布局方法，比如用，wrap_content,match_parent,weight来进行布局，尽量不直接使用固定数值的宽度和高度，优点:做一套布局，到处使用 缺点：需求精心布局
	-  将dp转为像素，如此一来就和屏幕的大小没有什么关系了，优点：复用性强 缺点：麻烦

-  为不同的屏幕创造不同的布局，使用限定符对布局进行修饰，比如：
	-  res/layout-large/main.xml： 默认布局
	-  res/layout/main.xml :系统会在属于较大屏幕（例如 7 英寸或更大的平板电脑）的设备上选择此布局
	-  res/layout-sw600dp/main.xml : 最小宽度为 600 dp 的屏幕会使用此布局

-  提供不同清晰度的位图：系统会根据设备的清晰度自动选择图片资源
<pre>
MyProject/
  res/
    drawable-xhdpi/
        awesomeimage.png
    drawable-hdpi/
        awesomeimage.png
    drawable-mdpi/
        awesomeimage.png
    drawable-ldpi/
        awesomeimage.png
</pre>

**skills**

-  如果使用 "wrap_content" 和 "match_parent" 尺寸值而不是硬编码的尺寸，您的视图就会相应地仅使用自身所需的空间或展开以填满可用空间。

-  请使用dp 和sp:
	-  请务必使用 dp 或 sp 单位指定尺寸。dp 是一种非密度制约像素，其尺寸与 160 dpi 像素的实际尺寸相同。sp 也是一种基本单位，但它可根据用户的偏好文字大小进行调整（即尺度独立性像素），因此您应将该测量单位用于定义文字大小（请勿用其定义布局尺寸）



**google 教程**

-  [Supporting Multiple Screens](https://developer.android.com/guide/practices/screens_support.html)
-  [支持各种屏幕尺寸](https://developer.android.com/training/multiscreen/screensizes.html)
-  [Supporting Different Screens](https://developer.android.com/training/basics/supporting-devices/screens.html)
-  [针对多种屏幕进行设计](https://developer.android.com/training/multiscreen/index.html)

**github**

-  [鸿洋的解决方案](https://github.com/hongyangAndroid/AndroidAutoLayout)