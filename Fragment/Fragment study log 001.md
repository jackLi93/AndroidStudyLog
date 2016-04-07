##Fragment study log 001


----
本文逻辑思路：

**什么是Fragment？**

**为什么要使用Fragment?**

**怎么使用Fragment？**

**最佳实践**

**核心要素**

**注意的问题：**

**拓展阅读：**

---

**什么是Fragment？：**


- Fragment表示Activity中的行为或者用户界面部分。可以将多个Fragment组合在一个Activity中来构建多窗格UI，以及在多个Activity中重复使用。
- Fragment有自己的生命周期，能够处理自己的输入事件。 
-  可以在Activity运行中添加和删除Fragment。
-  Fragment必须始终嵌入在Activity中使用，其生命周期直接受宿主Activity的影响。
-  Fragment分为包含UI的Fragment和不包含UI的Fragment,前者一般做用户界面并响应用户操作，后者一般做后台服务。

**为什么要使用Fragment？**

-  Android在3.0（API11）中引入了片段，主要是为了给大屏幕（如平板电脑）更加动态和灵活的UI设计提供支持。
-  最初是为了解决大屏幕适配的问题，现在Fragment越来越多的用在管理用户界面上，Activity只负责管理Fragment，而Fragment负责处理UI。
-  项目中常用的：Fragment与ViewPager的配合使用可以做出很多效果，比如：APP滑动的广告栏，微信的首页。

----

**怎么使用Fragment？**

- 如何使用Fragment是一个值得深究的问题，因为在开发中可能遇见各种意想不到的问题。我在使用的时候：就遇见了这样的问题：ViewPager与Fragment以及TabLayout配合想做出Tabs的效果，但是一开始Fragment的内容不显示或者显示不全，找了好久资料并未解决问题，后来看了官方文档重新写了一下Class文件，又能显示了，真奇怪。

**使用UI型的Fragment的一般步骤如下：**

-  定义Fragment 的布局文件，myFragment.xml;
-  继承Fragment新建一个类，MyFragment实现onCreate(),onCreateView,onPause()等方法。
	-  **系统会在片段首次绘制其用户界面时调用此方法。 要想为您的片段绘制 UI，您从此方法中返回的 View或者ViewGroup 必须是片段布局的根视图。**
	-  注意掌握Fragment的声明周期。
-  在 Activity 的布局文件内声明片段。
	-  注意：完整包名：<fragment android:name="com.example.news.ArticleListFragment" .../>
	-  也可以使用ViewPager等来动态管理Fragment。
-  或者通过编程方式将片段添加到某个现有 ViewGroup。也可以理解为动态添加删除Fragment。
-  在Activity中使用MyFragment。

**注意事项：**

1.  自定义Fragment的构造函数，Google不建议在构造函数内传入参数，而是使用另外一种方式传入参数。
	- 参看：[android中Fragment的构造函数](http://www.voidcn.com/blog/chuyouyinghe/article/p-4636706.html)

**最佳实践：**

请看拓展阅读部分。

**核心要素：**

-  掌握Fragment的生命周期
-  掌握Fragment与Activity的通信
-  掌握Fragment的View创建和用户交互
 
---

**注意的问题：**
 
1.  继承的Fragment的构造函数不能传入参数，需要传入参数得用Google给的另一种方法。
2.  Fragment内需要Context的时候可以调用：getActivity()。 
3.  可以写一个BaseFragment来添加我们自定义的一些方法和属性，然后其他所有Fragment继承BaseFragment。

**拓展阅读：**

-  [片段](http://developer.android.com/intl/zh-cn/guide/components/fragments.html)：Google开发者文档。

-  [Using ViewPager for Screen Slides](http://developer.android.com/intl/zh-cn/training/animation/screen-slide.html#fragment):Google官方文档，关于ViewPager与Fragment的应用。
-  [Building a Dynamic UI with Fragments](http://developer.android.com/intl/zh-cn/training/basics/fragments/index.html)

-  [知乎：关于Activity和Fragment](https://www.zhihu.com/question/39662488)
-  [5.2.4 Fragment实例精讲——底部导航栏+ViewPager滑动切换页面](http://www.runoob.com/w3cnote/android-tutorial-fragment-demo4.html)
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