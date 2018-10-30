### git 学习

#### 1. git add file1...   添加本地库文件到仓库，未提交状态
			git add -A  提交所有变化
			git add -u  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
			git add .  提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件
#### 2. git commit -m <message>  一次提交添加的文件们到仓库
#### 3. 版本修改 
		diff 相关知识：http://www.ruanyifeng.com/blog/2012/08/how_to_read_diff.html
		文件修改后，git status  结果会提示 modified:   readme.txt （该文件被修改）是否要提交更新等
		//diff difference
		git diff 文件名 结果提示：修改前文件（-文件名）与修改后文件（+文件名）的改动，'-'表示修改前的文件或删除的行，'+'反之。
		//添加 提交   
		git add file1
		git commit -m <message>

#### 4.时光穿梭
##### 4.1版本回退  git reset --hard HEAD^
			在Git中，用HEAD表示当前版本上个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，
			所以写成HEAD~100。
			注意：cmd控制台中换行符默认是^，所以在cmd中会换行出现more，
				解决：	git git reset --hard HEAD~ //默认是一次
						git git reset --hard HEAD~1
						git git reset --hard "HEAD^"
			 git reset --hard commit_id 可以回到任意版本（快照）
			 git log、git reflog  可查看commit_id
##### 4.2 管理修改
			git操作的及修改，add 加入修改的进入暂存区，commit 提交暂存区进入分支。
			忘记commit 只会提交暂存区的内容！
			
			git diff  是你工作区跟stage的比较，这个时候可以看你开发过程中修改了哪些内容
			git diff --cached 是看你stage区和仓库分支上的比较，你add后但是没有commit， 这个时候只是在stage中，可以确认下修改是否正确，
			如果正确无误可以commit合并到分支
			git diff  HEAD -- 文件名 是比较工作区与仓库分支的区别，可以比较修改后与仓库分支的该文件的区别。
			正常情况下 stage暂存区与仓库分支 内容一致。
			第一次修改后add，第二次修改后add，再一次提交会合并两次修改。
##### 4.3撤销修改
		场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
		场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，
		第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。
		场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
		4.4 删除文件与恢复
			工作区中文件被删除也算是一次修改。
			当工作区中文件被删除，有两种情况：一、此文件没用所以被删除，要在版本库中删除：命令git rm删掉（git add也可以其相同作用，添加修改），并且git commit。
			二、是误删，是可以在版本库中恢复文件，命令：git checkout -- 文件名
#### 5.远程仓库
##### 5.1 本地添加远程库
			github中新建仓库，名称如learngit；
			本地添加远程仓库，命令$ git remote add origin git@github.com:malikcheng/learngit.git ,origin是远程库的名字，当然也可以改成别的，常规origin一看就知道是远程库；
			下一步是把本地仓库内容push（推送）到空白远程库:$ git push -u origin master,-u 参数是Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令；
			以后本地做了提交，就可以通过命令$ git push origin master，把本地master分支的最新修改推送至GitHub
			
			第一次使用到问题：Please make sure you have the correct access rightsand the repository exists 或Permission denied (publickey).
			这是本地没有权限上传到github账户上，需要配置ssh，将本地的公钥放到GitHub的ssh key 里面。
##### 5.2 本地克隆远程库
			命令：git clone git@github.com:malikcheng/learngit.git
			
			git clone支持多种协议，除了HTTP(s)以外，还支持SSH、Git、本地文件协议等，下面是一些例子;
			
			
			$ git clone http[s]://example.com/path/to/repo.git
			$ git clone http://git.oschina.net/yiibai/sample.git
			$ git clone ssh://example.com/path/to/repo.git
			$ git clone git://example.com/path/to/repo.git
			$ git clone /opt/git/project.git 
			$ git clone file:///opt/git/project.git
			$ git clone ftp[s]://example.com/path/to/repo.git
			$ git clone rsync://example.com/path/to/repo.git
#### 6.分支管理
##### 6.1 创建分支、合并分支、 删除分支
			
			查看分支：git branch
			创建分支：git branch <name>
			切换分支：git checkout <name>
			创建+切换分支：git checkout -b <name>
			合并某分支到当前分支：git merge <name>
			删除分支：git branch -d <name>
##### 6.2 冲突解决
			当前分支与另一分支的文件内容不一致时，用快速合并（git merge 分支名）到当前分支有冲突出现，需要手动解决冲突。
			方法是：把git合并失败的文件编辑成我们希望的文件，在提交。
			merge 时错误信息：
			Auto-merging readme.txt
			CONFLICT (content): Merge conflict in readme.txt
			Automatic merge failed; fix conflicts and then commit the result.
			命令：git log --graph --pretty=oneline --abbrev-commit 查看分支的提交情况,按时间排序
##### 6.4分支管理策略
			合并某分支到当前分支：git merge <branchname>，是快速合并模式，如果没有冲突，只将head指针指向最后一次提交的当做master，这样删除分支后，会丢失分支信息
			新命令：git merge --no-ff -m <message> <branchname>,禁用快速合并模式并生成commit信息，这样就能在log看到merge时的信息
			
##### 6.5 Bug分支
			情景：以前上线版本或这其他以前负责的项目（只要符合修复bug内容和自己分支内容不冲突即可）出现了bug，紧急需要处理，但是自己在的dev分支工作还没有完成，不能随便切换分支哦 。
				在自己dev分支使用git stash（储藏）命令储藏现在分支的未完结的工作进度。此时，就可以切换master分支，建issue分支来处理bug后 add、commit，到master分支merge，删除issue分支；
					到自己的dev分支，git stash apply（恢复储藏的工作，stash保存工作进度；git stash pop 恢复储藏的工作，stash不记录保存 ）。git stash list 查所有保存工作进度
					git stash drop [stash_id] 删除储藏的进度
				小结：git stash 命令后，会隐藏当前进度。会回到上次提交后的状态。
##### 6.6 多人协作
			开发中多个人会在自己本地分支关联远程库master下dev分支，push自己的代码。
			这时会遇到两个人共同编辑一行代码，前一个人push成功，后一个人会push失败。
			此刻要pull回来，打开冲突文件处理冲突内容，才会push成功。
			相关命令：git fetch origin //刷新远程库分支信息
					git clone git@github.com:michaelliao/learngit.git
					git checkout -b dev origin/dev //创建本地分支dev 与远程库下dev分支关联
					E:\the other\learngit>git push origin dev  //push 时发生冲突
											To github.com:malikcheng/learngit.git
											 ! [rejected]        dev -> dev (non-fast-forward)
											error: failed to push some refs to 'git@github.com:malikcheng/learngit.git'
											hint: Updates were rejected because the tip of your current branch is behind
											hint: its remote counterpart. Integrate the remote changes (e.g.
											hint: 'git pull ...') before pushing again.
											hint: See the 'Note about fast-forwards' in 'git push --help' for details.
					git pull //把最新的提交从origin/dev抓下来在本地合并，解决冲突，再推送
											$ git pull //发生错误 原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：
												There is no tracking information for the current branch.
												Please specify which branch you want to merge with.
												See git-pull(1) for details.

													git pull <remote> <branch>

												If you wish to set tracking information for this branch you can do so with:

													git branch --set-upstream-to=origin/<branch> dev
											$ git branch --set-upstream-to=origin/dev dev //本地dev与远程dev建立链接
			
#### 7. 标签管理
#####  7.1 创建标签
					标签可以给每次的提交打上标签，如果没有指定commit id会指定当前的最新的一次提交。
					命令 git tag <commitId> tagname
					列出所有tag git tag //不是按时间排序，是按字母排序
					列出某标签的信息 git show tagname
					删除标签 git tag -d tagname
##### 7.2 操作标签
					标签能在本地存也能推送到远程库存放
						命令：git push origin tagname //一次推送一次标签
								git push origin tags  //一次推送全部没有推送过标签们
								
					推送到远程库删除标签比较麻烦，现在本地删除标签；然后，从远程删除。删除命令也是push，但是格式如下：git push origin :refs/tags/v0.9	
					在所在的repository里的releases里可以看到标签，地址https://github.com/MalikCheng（用户名）/learngit（项目）/tags
---	
最后补充几个常用命令：
		
			git remote show 展示远程库的名字
			git show 		展示本地库信息
			git branch -m newname 将本地所在分支赋予新名字
			git branch -m oldname newname 指定要重命名分支名字
			git remote rm origin 在本地仓库删除远程仓库
			git remote add origin git@github.com:malikcheng/learngit.git 添加远程库
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
	
			
			
			
			
			