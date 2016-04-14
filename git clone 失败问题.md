##Git clone 失败问题

---
1.  当用git clone url命令时，出现了uable acess to ...，port 443 time out的错误时：
**问题原因：**
	-  网络被限制，访问不了。比如我在学校就能直接克隆，但是在公司就不能直接访问。

 **解决办法：**

	-  设置代理
	-  git config --global http.proxy 127.0.0.1:8087
	-  注意：后面的参数是端口号，依据自己代理端口而定

**参考文档：**

-  [GitHub - failed to connect to github 443 windows/ Failed to connect to gitHub - No Error](http://stackoverflow.com/questions/18356502/github-failed-to-connect-to-github-443-windows-failed-to-connect-to-github)