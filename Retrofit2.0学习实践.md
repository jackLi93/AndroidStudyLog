###Retrofit2.0学习实践

----
**关于Retrofit**

-  Retrofit 目前Android上最流行，最便捷，最高效的网络库。
-  A type-safe HTTP client for Android and Java
-  Square开源的项目，目前版本2.0
-  [官网地址：Retrofit](http://square.github.io/retrofit/)
- ---

**Retrofit2.0的核心技术：**

1.  Retrofit将Http API转化为Java接口。
2.  Retrofit会自动生成API接口的实现类。
3.  每一个请求既可以是同步请求远程服务端也可以是异步请求远程服务端。
4.  广泛使用注解来完成Http请求，简化代码。如：@GET，@POST,@PUT,@HEADER等等。
5.  Retrofit2.0不再内置解析器，但是允许你添加解析器的插件然后给Retrofit使用。比如：Gason,Jackson,Moshi,Protobuf等。还可以自定义数据解析方法。这样一来使得Retrofit的灵活性大大增加。
 
  //But in Retrofit 2.0, Converter is not included in the package anymore. You need to plug a Converter in yourself or Retrofit will be able to accept only the String result. As a result, Retrofit 2.0 doesn't depend on Gson anymore.
 
---
**对上面核心技术的具体解释：**

1.  Retrofit turns your HTTP API into a Java interface.
<pre>
<code>
public interface GitHubService {
  @GET("users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user);
}

</code>
</pre>
**代码解析**

-  将Github的服务端的API封装为本地的一个GitHubService的接口，接口中有GET方法，该方法即向服务端发送GET请求。
-  使用了@GET的注解，@Path注解，前者表示GET请求，后者表示在当前目录下


2.The **Retrofit** class generates an implementation of the **GitHubService** interface. 
<pre>
<code>
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com/")
	.addConverterFactory(GsonConverterFactory.create()) 
    .build();

GitHubService service = retrofit.create(GitHubService.class);
</code>
</pre>
**代码解析：**

-  生成一个Retrofit的对象，调用.baseUrl绑定基础URL
-  使用Gason解析数据
-  retrofit.create()方法直接生成接口的实现类。

3.调用请求，可同步方式也可以异步方式：

Call<List<Repo>> repos = service.listRepos("octocat");


**代码解释：**

-  上面的一行代码即完成了对服务端get请求的调用。是不是很简单！！

---
以上三步就完成了对Retrofit的基本使用，Retrofit剩下的很大一部分就是对Retrofit注释的学习和使用，另外在实际开发中可能还需要对解析器的替换，也有可能涉及到自定义的解析器，参考官方文档去学习理解。

**注意：**所有的第三方包都需要在AndroidStudio中添加依赖：
<pre>
<code>
compile 'com.squareup.retrofit2:retrofit:2.0.0' //注意获取最新版本
</code>

</pre>

---

**Retrofit2.0的最佳实践**

**快捷工具：**

[jsonschema2pojo](http://www.jsonschema2pojo.org/):将Json数据快速转换为java.Class。

**展开阅读：**

-  [Retrofit 2.0: The biggest update yet on the best HTTP Client Library for Android](http://inthecheesefactory.com/blog/retrofit-2.0/en)
-  [Retrofit](http://www.jianshu.com/p/1ef0ba0bccc6)