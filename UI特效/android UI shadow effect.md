###android UI shadow effect

**Android界面的阴影效果**

-  在Android5.0中退出了Material Design ，里面介绍了关于Elevation and shadows 
	- 链接地址： [What is material?– Elevation and shadows](https://www.google.com/design/spec/what-is-material/elevation-shadows.html#)
	- 文章指出：在Material Design设计中，所有的Object都和现实世界中的Object相似，有各自的三围的位置，彼此可以相互覆盖，但不可想入穿过。因此各自也有阴影效果，且高度不同阴影效果也不一样。

**在github search android  shadow关键字，会出现很多相关的库：**

-  [ShadowViewHelper](https://github.com/wangjiegulu/ShadowViewHelper):这是一个非常容易使用的库，使用方法三步走：
	-  1.在Androidstudio中添加依赖
	-  2.在xml中布局
	-  3.在代码中找到布局，几行代码就能实现阴影效果

-  [ZDepthShadow](https://github.com/ShogoMizumoto/ZDepthShadow):Android - draw z-depth shadow of MaterialDesign
	-  这个使用起来就更加简单了，直接添加依赖后在xml布局中使用即可。 
