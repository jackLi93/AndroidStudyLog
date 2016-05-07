## 关于Mina:一个老的网络库

![Mina架构](http://dl.iteye.com/upload/picture/pic/49651/5bb8da4a-7714-3dc7-adfe-bc6450c635d4.jpg)


**Mina核心类**

-   IoService 是负责底层通讯接入，而 IoHandler 是负责业务处理的。
-    在上图中最右端也就是 **IoHandler，这便是业务处理模块**。在业务处理类中不需要去关心实际的通讯细节，**只管处理客户端传输过来的信息即可**。
-    为了简化 Handler 类，MINA 提供了 IoHandlerAdapter 类，此类仅仅是实现了 IoHandler 接口，但并不做任何处理。 

**注意**

-  虽然Mina是异步的网络框架，但是awaitUninterruptibly();  却是同步的方法，调用这个方法程序会堵塞，所以在android中使用的时候不要调用这个方法，而是使用：
-  connFuture.addListener来进行异步监听

<pre>
1. 同步代码
ConnectFuture future = connector.connect(address);  
future.awaitUninterruptibly();  
</pre>

<pre>
2.异步代码
ConnectFuture connFuture = connector.connect(address);  
connFuture.addListener(new ConnectListener());  
        private class ConnectListener implements IoFutureListener<ConnectFuture>{  
              
            public void operationComplete(ConnectFuture future) {  
                  
                if (future.isConnected()) {  
                    //get session  
                    IoSession session = future.getSession();  
                      
                    session.write(...);  
                      
                } else {  
                      
                    logger.error("can not create the connection .");  
                }  
            }  
        }  
    }  
} 
</pre>

**程序Crash**

-  运行APP的时候，Crash，提示信息为：
	05-06 10:23:57.805: E/AndroidRuntime(2368): java.lang.IllegalArgumentException: message is empty. Forgot to call flip()?


**展开阅读**

-  [异步机制（Asynchronous） -- （一）开篇兼谈Mina](http://blog.csdn.net/historyasamirror/article/details/6159233)
-  [Java Code Examples for org.apache.mina.core.future.ConnectFuture.addListener()](http://www.programcreek.com/java-api-examples/index.php?class=org.apache.mina.core.future.ConnectFuture&method=addListener)