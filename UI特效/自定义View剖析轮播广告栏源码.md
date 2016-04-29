##自定义View剖析轮播广告栏源码

**自定义View的分类**

从实践的角度来对自定义View进行分类，可以大致分为以下的四类：

-  继承View
-  继承ViewGroup
-  继承特定的view控件，比如TextView，来添加一些功能
-  继承自LinearLayout，RelativeLayout等

---

**实例剖析：**

现在需要实现一个可扩展性的首页轮播控件，基于对ViewPager的扩展：
<pre>
1.继承ViewPager，通过反射方式实现支持低版本上的切换动画,实现是否可供滑动

public class BGAViewPager extends ViewPager{
	//boolean的变量用于标识是否可滑动。一般涉及到控件属性的变量值都可以在成员变量中进行声明。
	private boolean mScrollable = true;
	
	public BAGViewPager(Context context){
		super(contxt);
	}
	/*
		在自定义View中此构造函数，是必须的，在此构造函数中对自定义的属性进行管理。BAGViewPager没有添加自定义属性，故只是调用了父类的构造函数。
	*/
	public BAGViewPager(Context context,AttributeSet attrs){
	super(context,attrs);
	}
	
	@Override
	public void setPageTransformer(boolean reverseDrawingOrder,ViewPager.PageTransformer transformer){

		Class viewPagerClass = ViewPgaer.class;
	
	try{
		boolean hasTransformer = transformer!=null;
		Field pageTransformerField = viewPgaerClass.getDeclaredField("mPageTransformer");
		pageTransformerField.setAccessible(true);
		PageTransformer mPageTransformer = (PageTransformer)pageTransformerField.get(this);

		boolean needsPopulate = hasTransformer != (mPageTransformer !=null);

		pageTransformerField.set(this,transformer);
	
		Method setChildrenDrawingOrderEnabledCompatMethod = viewPagerClass.getDeclaredMethod("setChildrenDrawingOrderEnabledCompat", boolean.class);
		setChildrenDrawingOrderEnabledCompatMethod.setAccessible(true);
		setChildrenDrawingOrderEnabledCompatMethod.invoke(this,hasTransformer);
		Field drawingOrderField = 	viewPagerClass.getDeclaredField("mDrawingOrder");
		drawingOrderField.setAccessible(true);
	if(hasTransformer){
		drawingOrderField.setInt(this,reverseDrawingOrder?2:1);
	}else{
		drawingOrderField.setInt(this,0);	
	}
	if(needsPopulate){
		Method populateMethod = viewPagerClass.getDeclaredMethod("populate");
		populateMethod.setAccessible(true);
		populateMethod.invoke(this);
	
	}	

	}catch(Exception e){
		e.printStackTrace();
	}

	}

	//扩展ViewPager添加的方法，暴露给User是否允许滑动
	public void setAllowUserScrolable(boolean scrollable){
	
	mScrollable = scrollable;
	}
	//接下来就是对滑动事件的拦截处理,是否拦截事件，如果是，则会对滑动事件进行处理，如果否，则不处理。
	@Override
	public boolean onInterceptTouchEvent(MotionEvent ev){
	if(mScrollable){
		return super.onInterceptTouchEvent(ev);	
	}else{
		return false;
	}
	}

	@Override
	public boolean  onTouchEvent(MotionEvent ev){
	
	if(mScrollable){
		return super.onTouchEvent(ev);	
	}else{
		return false;	
	}				
	}
	
	//相对ViewPager拓展一个新的方法用于给User设置page切换的时间
	//此处使用了反射的技术
	public void setPageChangeDuration(int duration){
		try{
			Field scrollerField = ViewPager.class.getDeclaredField("mScroller");
			scrollerField.setAccessible(true);
			srcollerField.set(this,new PageChangeDurationScroller(getContext(),duration));
			}catch(Exception e){
			
			e.printStackTrace();	
		}
	
	}
}

</pre>
**注：**以上代码皆为手动敲写，未使用编译器，未导入类，只为熟悉代码编写思路，难免有错。

**拓展阅读**

-  关于反射：[Java编程 的动态性，第 2部分: 引入反射](https://www.ibm.com/developerworks/cn/java/j-dyn0603/)

**项目地址**

-  [BGABanner-Android](https://github.com/bingoogolapple/BGABanner-Android)