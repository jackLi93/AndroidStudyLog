##CardView
**CardView**
-  CardView是Material Deign库中的一个卡片式控件，常常和RecyclerView 联合使用。

**效果图**

![使用卡片的效果](https://developer.android.com/design/material/images/card_travel.png)

**使用方法**
-  第一步添加依赖
-  第二步xml中布局

<pre>
<code>
dependencies {
    ...
    compile 'com.android.support:cardview-v7:21.0.+'
    compile 'com.android.support:recyclerview-v7:21.0.+'
}

</code>
</pre>

 		<!-- A CardView that contains a TextView -->
 		<android.support.v7.widget.CardView
        xmlns:card_view="http://schemas.android.com/apk/res-auto"
        android:id="@+id/card_view"
        android:layout_gravity="center"
        android:layout_width="200dp"
        android:layout_height="200dp"
        card_view:cardCornerRadius="4dp">

        <TextView
            android:id="@+id/info_text"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />
    </android.support.v7.widget.CardView>


** issues**

-  CardView中包含ImageView,然后图片沾满CardView的解决办法：

 	[Make ImageView fit width of CardView](http://stackoverflow.com/questions/27394300/make-imageview-fit-width-of-cardview)

**展开阅读**

-  [关于使用 CardView 开发过程中要注意的细节](http://blog.feng.moe/2015/10/24/something-about-cardview-development/)
-  [官方教程：创建列表与卡片](https://developer.android.com/training/material/lists-cards.html)