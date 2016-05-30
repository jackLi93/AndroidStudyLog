##关于更新Fragment里数据&UI的问题

**问题根源**

-  一般来说，Fragment只能与其父Activity进行通信，而不可直接与其他的Activity进行通信

**情景再现**

- 此次遇见的问题是：在MainActivity有一个ViewPager管理Fragment，在Fragment中有一个编辑房间的按钮，点击按钮，开启了一个新的Activity，并在新的Activity中对传入的房间对象的数据进行修改。修改结束返回当前Fragment，但问题是：数据修改后，当前Fragment的UI并没有更新

**解决思路**

-  在MainActivity中获取当前Fragment的对象，然后调用自定义的业务方法更新UI或者重新加载此Fragment。

**流程和代码**

-  1.Fragment中跳转Activity使用startActivityForResult；
	-  展开阅读：[Android------startActivityForResult的详细用法](http://blog.csdn.net/sunchaoenter/article/details/6612039)
-  2.在MainActivity的onActivityResult对返回的code进行判断，如果是与一对应的Activity返回的结果，就获取当前Fragment对象并重新载入。
 	- 获取Activity中当前Fragment对象[Getting the current Fragment instance in the viewpager](http://stackoverflow.com/questions/18609261/getting-the-current-fragment-instance-in-the-viewpager)
 	- 伪代码像这样：
<pre>
方法一：
 int index = vpPager.getCurrentItem();
 MyPagerAdapter adapter = ((MyPagerAdapter)vpPager.getAdapter());
 MyFragment suraVersesFragment = (MyFragment)adapter.getRegisteredFragment(index);//获取当前Fragment对象
suraVersesFragment.udpateUI()；//更新UI，此方法为自定义的方法与业务逻辑相关

----分界线---
方法二：使用findFragmentByTag来获取Fragment对象。
case R.id.addText:
     Fragment page = getSupportFragmentManager().findFragmentByTag("android:switcher:" + R.id.pager + ":" + ViewPager.getCurrentItem());
     // based on the current position you can then cast the page to the correct 
     // class and call the method:
     if (ViewPager.getCurrentItem() == 0 && page != null) {
          ((FragmentClass1)page).updateList("new item");     
     } 
return true;

---分界线---
方法三：重新加载Fragment：
Fragment frg = null;
frg = getSupportFragmentManager().findFragmentByTag("Your_Fragment_TAG");
final FragmentTransaction ft = getSupportFragmentManager().beginTransaction();
ft.detach(frg);
ft.attach(frg);
ft.commit();
</pre>

**异常处理**

在使用方法一的时候得到的返回码为65537：

-  问题原因：
-  解决办法：Change:

	startActivityForResult(intent, 1);

	To:

	getActivity().startActivityForResult(intent, 1);

-  参考答案：[wrong requestCode in onActivityResult](http://stackoverflow.com/questions/10564474/wrong-requestcode-in-onactivityresult)
 
**最佳解决方案**

-  以上三种方法的实践中总是出现各种问题，虽然思路上是可行的，经过思考我进行了如下的方法特别容易的解决了该问题，而且没有bug。
-  首先，定义一个Handler，
<pre>
    public Handler uiHandler = new Handler(){
        @Override
        public void handleMessage(Message msg) {
            //super.handleMessage(msg);
            if(msg.what==1){
                updateUI();
            }
        }
    };
</pre>

-  第二步：定义updateUI()的方法：
<pre>
    public  void updateUI(){
       getDbRooms(getActivity());
        initView(listRoom);
        room_bg.setImageResource(listRoom.get(ClickPosition).getImg());
        //this.onActivityCreated();
    }

</pre>
-  第三步：重新Fragment的onResume()的方法：
<pre>
    @Override
    public void onResume() {
        super.onResume();
        uiHandler.sendEmptyMessage(1);
    }
</pre>
-  这种方法思路清晰，操作简便，没有什么太大的bug，而且经过我的验证结果可行。

---
**展开阅读**

-  这里是使用viewPager的适配器更新Fragment的数据[Updating ViewPager With New Data Dynamically](http://www.tothenew.com/blog/updating-viewpager-with-new-data-dynamically/)
-  [Refresh Current Fragment within ViewPager | Android Tutorial and Example](https://www.youtube.com/watch?v=8wAZ3uWioTk)
-  [Android: refresh/update fragment from another activity on button clicked](http://stackoverflow.com/questions/33820301/android-refresh-update-fragment-from-another-activity-on-button-clicked)
- codepath的教程：[Creating and Using Fragments](https://github.com/codepath/android_guides/wiki/Creating-and-Using-Fragments)
- [Getting the current Fragment instance in the viewpager](http://stackoverflow.com/questions/18609261/getting-the-current-fragment-instance-in-the-viewpager)