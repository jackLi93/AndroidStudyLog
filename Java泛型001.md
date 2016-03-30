###泛型
---
**关于泛型**

-  JAVASE5带来了泛型的特性
-  泛型实现了参数化类型的概念，极大的提高了程序的灵活性
-  很多原因促成了泛型的出现，很大一个原因是为了创造容器类，List，Map等用来作为数据容器的实现类。
- 看一些代码：
<pre>
<code>
1.  这段代码展示如何持有一个对象，即单个数据的容器
class User{};//这是一个数据类

public class UserHolder1{
	private User mUser;
	public UserHolder1(){}
	public UserHolder1(User user){
		this.mUser=user;
	}
	public void setUser(User user){
		this.mUser = user;
	}
	public User getUser(){
	return mUser;
	}

}
</code>
</pre> 
上面这些代码实现了数据容器类，但是可重用性不怎么样，它只能持有User的类，而无法持有其他类型的任何对象，然而我们并不想为每一个数据类都编写一个新的Holder类。

-  在Java 5之前我们可以直接让这个类持有Object对象：
<pre>
<code>
2.  在Java5之前，这样写一个dataHolder.class。
	public class dataHolder{
	private Object data;
	public dataHolder(){}
	publi dataHolder(Object data){
		this.data = data;
	}
	public Objetc getData(){
		return data;
	}
	public void setData(Object data){
		if(data!=null){
		this.data =data;
	}
	}
	public static void main(String []args){
		dataHolder holder = new dataHolder(new User());
		User data = (User)holder.getData();
		holder.set("hello world");
		String datas  = (String)holder.getData();
		holder.setData(66);
		Integer dataInt = (Integer)holder.getData();
	}
	}
</code>
</pre>
以上代码是Java 5之前的写法，可以存储任何类型的对象。首先，这样写设计到类型的强制转换，可能丢失数据。另外，通常而言一个数据容器一般用来存储一种类型的对象。所以泛型的主要目的之一就是用来指定数据容器到底要持有什么类型。**泛型其实有点像是一个类型的占位符，在定义数据结构的时候，我们只知道这个地方在未来会替换为一个具体的数据类型，但是具体什么数据类型，我们不知道，只有使用者知道。**

<pre>
<code>
3.使用泛型的dataHolder的写法
	
	public class dataHolder &lt T &gt{
		private T data;
		public dataHolder(){}
		public dataHolder（T data){
			this.data = data;
		}
		public T getData(){
			return data;
		}
		public void setData(T data){
			this.data = data;
		}
		public static void main(String args){
			dataHolder<User> userData = new dataHolder<User>(new User());
			User data = userData.getData();
			//userData.set("hello world");//error
			//userData.set(66);//error;
			//Note： userData can only hold User instance；
		}
	}	

</code>
</pre>
**注意：**这里的T就是类型参数，可以用其他大写字母代替，java源文件中用的E。 

####泛型的使用

-  泛型的使用包含泛型类，泛型接口 和泛型方法。

-  泛型接口和泛型类的使用方法没有太大的区别。
-  泛型方法使得该方法能够独立于类而产生变化。无论何时，只要你能做到，应该尽量使用泛型方法。
-  定义泛型类和泛型接口在类名或者接口名后加<T\>。比如，Node<T>，List<T>...
-  定义泛型方法只需要将泛型参数列表置于返回值之前。像下面这样：
<pre>
<code>
...
	public class Test{
	//输入不同的对象，会打印出其完整包名。
	public &lt T &gt void f(T x){
		System.out.println(x.getClass.getName());	
	};
	看一个泛型方法和可变参数的例子：
	public static &lt T &gt List &lt T &gt  makeList(T... args){
		ArrayList<T> list = new ArrayLisr<T>();
		for(T item: args){
			list.add(item);
		}
	return list;
	}
...
}
</code>
</pre>
**注意：**

1.  是否拥有泛型方法与其所在的类是否是泛型没有关系。即一个类不是泛型类，但是可以有泛型方法。
2.  在使用泛型类的时候，必须指定类型参数，而在使用泛型方法的时候，不需要指定类型参数，因为编译器会帮我们找到具体的类型。