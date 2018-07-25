#git命令总结

##配置

	$ git config --global user.name "Your Name"
	$ git config --global user.email "email@example.com"

表示你这台机器上**所有的Git仓库**都会使用这个配置（当前用户根目录下的.gitconfig）,如果只想对某一个项目生效，需要在项目的/.git/config文件中修改。

	[user]
    	name = Your Name
   		email = your@email.com


##工作区和版本库

![图片来自http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0](Git_Repository.jpeg)

其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

	git add <file> // 把文件放到暂存区（add to stage or index）
	
	git commit -m "xxx" //提交更改，实际上就是把暂存区的所有内容提交到当前分支。

	git checkout -- file //丢弃工作区的修改 --很重要，没有的话就变成切换分支了
	
	git reset HEAD file  //把暂存区的修改撤销掉（unstage），重新放回工作区
	
	git status 		//可以知道工作区哪些文件修改了
	
	git diff <file> //查看指定文件的修改内容
	
##远程仓库

origin指向的就是你本地的代码库托管在远程Git仓库


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
	
	所有的分支都是本地的分支库备份，如果之前没有git-fetch，看不到别人刚刚提交的分支。
	
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

##多人协作的工作模式

>首先，可以试图用推送自己的修改；

	git push origin local_branch_name

>如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
	
	git pull
	
>如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令

	git branch --set-upstream local_branch_name origin/remote_branch_name。

>如果合并有冲突，则解决冲突，并在本地提交；

>没有冲突或者解决掉冲突后，再用git-push推送就能成功！

	git push origin branch-name
	
##标签的使用
发布版本的时候，我们通常会在版本库中打一个标签（TAG）。标签相当于版本库的一个快照。
实际上就是指向某个commit的指针，且不能移动，只能创建和删除。相当于commit_id的别名，方便归档。

>创建Tag，用-a指定标签名，-m指定说明文字.还可以指定在某个commit上打标签，默认为HEAD
	
	git tag -a <tagname> -m "blablabla..." [HEAD|commit_id] 
	
>推送某个标签到远程，使用命令
	
	git push origin <tagname>	
	
>删除标签

	git tag -d <tagname>

>删除远程标签

	git push origin :refs/tags/<tagname>
	





	 
	
