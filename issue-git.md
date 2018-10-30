# 错误记录
## 1. git push发生错误
```
	E:\the other\learngit>git push origin dev
	fatal: remote error:
	  You can't push to git://github.com/MalikCheng/learngit.git
	  Use https://github.com/MalikCheng/learngit.git
```
 
 解决：	如果在git clone的时候用的是git://github.com:xx/xxx.git 
			的形式, 那么就会出现这个问题，因为这个protocol是不支持push的。
			方案1：所以可以删除本地clone的库重新clone
					$ git remote rm origin  
					$ git remote add origin git@github.com:user_name/user_repo.git  
					$ git push origin  
			方案2：直接找到.git文件（有时可能是隐藏文件）——用记事本打开config，将url内容改成如下格式即可：	