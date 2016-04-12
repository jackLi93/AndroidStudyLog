####在来回跳转Fragment时出现闪退的问题：

-----
**问题描述：**
	
- 项目中需求模式和控制两个功能的切换，于是就用两个Fragment来进行相互的切换，但是在来回切换的过程中，发现该Activity直接闪退，界面返回至上一个Activity。

**问题排查：**
	
-  那么我刚开始以为是内存OOM，但是在查看内存之后发现并不是OOM。然后我感觉应该是在Fragment的切换代码中有问题，于是上网查询。如下链接和我的问题类似：
-  [ Fragment在切换Activity后返回，生命周期突然走向Destroy](http://www.eoeandroid.com/thread-271514-1-1.html)
	-  问题出在FragmentTransaction事务管理上

-  [Android Fragment Transaction Commits Crashes App](http://stackoverflow.com/questions/23798669/android-fragment-transaction-commits-crashes-app)
-  [管理Fragments](http://www.cnblogs.com/mengdd/archive/2013/01/09/2853254.html)
-  [官方培训文档](http://developer.android.com/intl/zh-cn/guide/components/fragments.html)

**解决问题：**

-  最后问题的解决是在看了官方培训文档后，发现其中多了一行代码：transaction.addToBackStack(null);

-  官方代码：
<pre>
	
// Create new fragment and transaction
Fragment newFragment = new ExampleFragment();
FragmentTransaction transaction = getFragmentManager().beginTransaction();

// Replace whatever is in the fragment_container view with this fragment,
// and add the transaction to the back stack
transaction.replace(R.id.fragment_container, newFragment);
transaction.addToBackStack(null);//应该有的一行代码

// Commit the transaction
transaction.commit();	
</pre>

-  修改后能完整运行的部分代码：
-  <pre>

		mFragmentLayout= (FrameLayout) findViewById(R.id.fragment_layout);

        modelFragment=new ModelFragment();
        controlFragment=new ControlFragment();
        mFragmentManager=getSupportFragmentManager();
        FragmentTransaction fragmentTransaction=mFragmentManager.beginTransaction();
        fragmentTransaction.replace(R.id.fragment_layout, controlFragment);
        //fragmentTransaction.add(controlFragment,"controlFragment");
        fragmentTransaction.show(controlFragment);
        fragmentTransaction.commit();

        mModelTextView=(TextView)findViewById(R.id.model_textview);
        mModelTextView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mModelTextView.setBackgroundColor(Color.parseColor("#a9b7b7"));
                mControlTextView.setBackgroundColor(Color.parseColor("#f5f5f5"));
                //mFragmentManager.popBackStack();
                FragmentTransaction fragmentTransaction=mFragmentManager.beginTransaction();
              fragmentTransaction.replace(R.id.fragment_layout, modelFragment);
                fragmentTransaction.addToBackStack(null);
                fragmentTransaction.commit();
            }
        });
        mControlTextView=(TextView)findViewById(R.id.control_textview);
        mControlTextView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                mControlTextView.setBackgroundColor(Color.parseColor("#a9b7b7"));
                mModelTextView.setBackgroundColor(Color.rgb(245, 245,245));
                //mFragmentManager.popBackStack();
                FragmentTransaction fragmentTransaction = mFragmentManager.beginTransaction();
                fragmentTransaction.replace(R.id.fragment_layout, controlFragment);
                //fragmentTransaction.add(controlFragment,"controlFragment");
                fragmentTransaction.addToBackStack(null);
                fragmentTransaction.commit();
            }
        });
-  </pre>