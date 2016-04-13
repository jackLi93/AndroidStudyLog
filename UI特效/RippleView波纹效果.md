####RippleView波纹效果

---

-  一个实现了 Android L 上才引入的点击按钮后出现水波纹效果的按钮
-  项目地址：[RippleView](https://github.com/siriscac/RippleView)

**RippleEffect**

-  一个实现 Material Design Ripple 效果的库，支持 Android API 9+以上版本。
-  项目地址：[RippleEffect](https://github.com/traex/RippleEffect)
-  我folk的地址：[RippleEffect](https://github.com/jackLi93/RippleEffect)

**国人整理的Android开源项目集：**

-  [android-open-project](https://github.com/Trinea/android-open-project)  
-  包含了很多UI特效，基本上现在市场上能见的效果都能在里面找到。
-  包含很多工具库:比如网络，图片，数据库等。。。

**妹纸**

-  每天一张精选妹纸图，一个精选小视频，一篇精选程序猿干货，数据来自 gank.io.

**波纹效果的最佳实践：**

-  使用[RippleEffect](https://github.com/jackLi93/RippleEffect)这个库。
-  第一步：添加依赖：
	-  dependencies {
    compile 'com.github.traex.rippleeffect:library:1.3'
}
-  第二步：在代码里布局：

 		<com.andexert.library.RippleView
        android:id="@+id/more"
        android:layout_width="?android:actionBarSize"
        android:layout_height="?android:actionBarSize"

        app:rv_color="#666666"
        android:layout_margin="5dp"
        rv_centered="true">

        <ImageView
            android:layout_width="?android:actionBarSize"
            android:layout_height="?android:actionBarSize"
            android:src="@android:drawable/ic_menu_edit"
            android:layout_centerInParent="true"
            android:padding="10dp"
            />
	 </com.andexert.library.RippleView>

	-  **注意:**在RippleView中可以包含各种xml布局，比如ImageView等、
    
-  第三步：自定义属性，比如波纹的颜色：app:rv_color="#666666"
	-  **注意：**自定义属性要想产生效果，需要添加自定义的命名空间：
		-  xmlns:app="http://schemas.android.com/apk/res-auto"