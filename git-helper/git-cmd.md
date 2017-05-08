#git命令总结

origin指向的就是你本地的代码库托管在Github上的版本

##创建一个本地分支

	git branch local_branch_name

创建一个和当前分支一样的本地分支

##推送一个本地分支到远程分支

	git push origin local_branch_name
	
##删除本地分支

	git branch -d branch_name
		(-d Shortcut for --delete)
	git branch -D branch_name 
		(-D Shortcut for --delete --force.)
	
##查看当前分支的状态

	git status	

##查询分支

	git branch -a 查看所有分支
	git branch -v 查看所有本地分支
		（-v包含了最近提交的内容,-vv则会打印上游分支）
	git branch -r 查看所有远程分支
	
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
	
	


	 
	
