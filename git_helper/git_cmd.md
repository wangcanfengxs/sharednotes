#git命令总结

##配置

	$ git config --global user.name "Your Name"
	$ git config --global user.email "email@example.com"

表示你这台机器上**所有的Git仓库**都会使用这个配置

##工作区和版本库

![图片来自http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0](Git_Repository.jpeg)

其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

	git add <file> // 把文件放到暂存区（add to stage or index）
	
	git commit -m "xxx" //提交更改，实际上就是把暂存区的所有内容提交到当前分支。

	git checkout -- file //丢弃工作区的修改 --很重要，没有的话就变成切换分支了
	
	git reset HEAD file  //把暂存区的修改撤销掉（unstage），重新放回工作区
	
	git status 		//可以知道工作区哪些文件修改了
	
	git diff <file> //查看指定文件的修改内容
	

##创建一个本地分支

	git branch local_branch_name

创建一个和当前分支一样的本地分支

##推送一个本地分支到远程分支

	git push origin local_branch_name
	
origin指向的就是你本地的代码库托管在远程Git仓库
	
##删除本地分支

	git branch -d branch_name
		(-d Shortcut for --delete)
	git branch -D branch_name 
		(-D Shortcut for --delete --force.)
	
	
##查询分支

	git branch -a 查看所有分支
	git branch -v 查看所有本地分支
		（-v包含了最近提交的内容,-vv则会打印上游分支）
	git branch -r 查看所有远程分支
	
##查看当前分支的状态

	git status 		//可以知道哪些文件修改了
	git diff <file> //查看指定文件的修改内容
	
##切换到某个分支，分支已经在本地

	git checkout branch_name
	
##切换到远程分支
	
	git checkout -b local_branch_name origin/remote_branch_name
	
这个相当于：
	
	git branch local_branch_name remote_branch_name
	git checkout local_branch_name
	
	
作用是checkout远程的dev分支，在本地起名为dev分支，并切换到本地的dev分支
	
	还有一种方式，首先运行 git fetch，然后运行 git checkout branch_name

##git-pull的使用

git pull --help的介绍：
	从另一个仓库（origin，可以理解为远程分支）或者一个本地分支获取并且合并到当前本地分支。git pull 相当于 git fetch, git merge FETCH_HEAD。
	
从远程对应的（remote-tracking）分支合并到本地
	git pull, git pull origin 

从远程指定的分支合并到本地
	git pull origin remote_branch_name
	相当于，git fetch origin
			git merge origin/remote_branch_name
			

##git-push的使用

更新远程分支的内容


	git oush origin
	
	git push (默认就是origin)
	
	
##回退到之前的某一个版本

>查看当前分支的提交记录

	git log [--pretty=oneline]
	
	git reflog //获取每次HEAD的修改，帮助找回commit_id

>回退到指定版本号

	git reset [--hard] commit_id

在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

	
	


	 
	
