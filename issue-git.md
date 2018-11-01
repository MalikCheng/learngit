# 错误记录
---
## 1. git 遇到问题：Please make sure you have the correct access rightsand the repository exists 或Permission denied (publickey)

遇到这类问看看网上说一点都不详细，所以经过一番研究终于搞定，想给像我这样的童鞋给一篇仔细的解决办法

**今天第一下载完 Git-2.11.1-64-bit.exe最新版本，想从Git库中克隆项目。没想到原来要进行密钥生成。就是和你https://github.com
上的账号进行验证。在本机生成密钥与自己账号绑定。这样就可以从git上下项目了**


----------
解决方法如下：

 - 进入https://github.com/settings/profile   
 查看自己的账号和邮箱，记到记事本下来，下面会用到。
 
![这是账号名字](https://img-blog.csdn.net/20170213194852303?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzQyOTE3Nzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![自己的邮箱](https://img-blog.csdn.net/20170213194810849?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzQyOTE3Nzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
	
 - 打开Git输入命令git config --global user.name "yourname"回车
git config --global user.email“your@email.com"回车
![这里写图片描述](https://img-blog.csdn.net/20170213195522968?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzQyOTE3Nzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
$ ssh-keygen -t rsa -C "your@email.com"（请填你设置的邮箱地址）回车

接着出现：
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):

无视这些请继续直接按下回车

 - 直到出现
The key's randomart image is:
+---[RSA 2048]----+
|         ==++. . |
|      . ++.o .  .|
|     ..o++Oo     |
+----[SHA256]-----+![这里写图片描述](https://img-blog.csdn.net/20170213195627978?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzQyOTE3Nzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
这样的
 - 之后打开提示的目录下记事本打开id_rsa.pub，复制里面内容。
 - 进入自己的账号https://github.com/settings/keys      点击 New sshKey
 ![这里写图片描述](https://img-blog.csdn.net/20170213195735518?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzQyOTE3Nzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


----------

 - 复制的内容粘贴到Key里，Title可以不写，亲测。

![这里写图片描述](https://img-blog.csdn.net/20170213200646672?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzQyOTE3Nzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 - 验证：$ ssh -T git@github.com回车 看到
Hi MalikCheng! You've successfully authenticated, but GitHub does not provide shell access.
表示成功了！！

---
## 2. git push发生错误


```shell

	E:\the other\learngit>git push origin dev
	fatal: remote error:
	  You can't push to git://github.com/MalikCheng/learngit.git
	  Use https://github.com/MalikCheng/learngit.git	  
```
 

### 解决：	
			如果在git clone的时候用的是git://github.com:xx/xxx.git 
			的形式, 那么就会出现这个问题，因为这个protocol是不支持push的。
#### 	方案1：所以可以删除本地clone的库重新clone
					$ git remote rm origin  
					$ git remote add origin git@github.com:user_name/user_repo.git  
					$ git push origin  
####	方案2(未测试)：直接找到.git文件（有时可能是隐藏文件）——用记事本打开config，将url内容改成如下格式即可：	
		[remote"origin"]
		url = git@github:用户名/项目名.git