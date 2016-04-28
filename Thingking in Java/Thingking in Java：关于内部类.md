##Thingking in Java：关于内部类

---

![内部类](https://www.google.com/url?sa=i&rct=j&q=&esrc=s&source=images&cd=&cad=rja&uact=8&ved=0ahUKEwiX1eTs87DMAhWPcI4KHZPwCVwQjRwIBw&url=http%3A%2F%2Fwww.slideshare.net%2Fsoftwaredesigner%2Fsun-java&psig=AFQjCNGFDO86OoqPdnB4uHFVDE-AJu7Glw&ust=1461918170777327)

**什么是内部类？**

-  顾名思义，可以将一个类的定义放在另一个类的定义内部，这就是**内部类**
-  **注意：**一个java文件中最多只有一个Public的Class，但是可以有多个Class类，比如很多内部类。

**内部类分类**

-  内部类分为:一般内部类和static内部类。
-  一般的内部类：持有对外部类的**强引用**，具有对外部类所有元素的访问权限。但需要注意的是，正是因为内部类对外部类有强引用，所以：处理不好会产生**内存泄漏**的问题。我在Android项目开发中就遇见过此类的问题：一个实现了Runnable的内部类，然后在Activity来回跳转中，资源未来得及释放，内存泄漏最终导致内存溢出。


**为什么需要内部类**

-  一般来说，内部类继承之某个类（比如继承baseAdapter）或者是实现某个接口，比如实现很多Listener。往往有Listener的地方都会涉及到观察者模式，使用Listener来解决很多复杂的问题。

使用内部类必须回答的一个问题：

-  如果只是需要一个对接口的引用，为什么不通过外部类来实现那个接口呢？比如DataChangdListener。
	
	答案是：如果这能满足需求，那么就应该通过外部类来实现接口，就像Android开发中实现的View.OnClickListener一样。

-  每一个内部类都能独立的继承或者实现一个接口，所以外部类是否已经继承了或实现了某个接口，对于内部类是没有影响的。
-  如果没有内部类提供的，可以继承多个具体的或者抽象的类的能力，一些设计与编程问题就很难解决。从这个角度来看，内部类使得多重继承的解决方案变得完整。接口解决了部分问题，而内部类有效的实现了多重继承。

**内部类的使用场景**

-  Android控件中的所有点击事件：使用**匿名内部类**实现
-  Android中ViewPager,ListView等控件的Adapter，可以通过内部类继承BaseAdapter。当然这个类也可以放在外部独立成为一个类。

**个人感悟**

-  其实，对于内部类，我目前所能感受到其强大的地方，就是在外部类中去实现各种Listener接口。我们知道，使用Listener的场景都是**观察者模式**，而观察者模式的出现就是为了解决很多复杂的问题，比如：数据的更新，状态的改变，Android中控件的点击事件等。
-  **很多时候，我们直接使用匿名内部类进行对各种Listener的实现，来完成各自的业务逻辑。**
-  看些简单的例子：

<pre>
1.使用android点击事件的例子：用了匿名内部类
...
      TextView  addRouterTextView=(TextView)contentRoot.findViewById(R.id.add_router);
        addRouterTextView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent=new Intent(getActivity(),EsptouchDemoActivity.class);
                startActivity(intent);
                 popupWindow.dismiss();

            }
        });
...
</pre>

<pre>
2.自定义控件中，自己定义的一些接口，然后在应用层使用匿名内部类的方式来完成各自的业务逻辑。
...
final QuickAction quickAction = new QuickAction(getActivity(), QuickAction.VERTICAL);
        quickAction.addActionItem(addItem);
        quickAction.addActionItem(scanItem);
        //Set listener for action item clicked
        quickAction.setOnActionItemClickListener(new QuickAction.OnActionItemClickListener() {
            @Override
            public void onItemClick(QuickAction source, int pos, int actionId) {
                //here we can filter which action item was clicked with pos or actionId parameter
                //ActionItem actionItem = quickAction.getActionItem(pos);
                //Toast.makeText(getActivity(), actionItem.getTitle() + " selected"+pos, Toast.LENGTH_SHORT).show();
                if(pos==0){
                    Intent intent=new Intent(getActivity(),EsptouchDemoActivity.class);
                    startActivity(intent);
                }else if(pos ==1){
                    new IntentIntegrator(getActivity()).setCaptureActivity(ToolbarCaptureActivity.class).initiateScan();
                }
            }
        });
...
</pre>