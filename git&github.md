##Git安装好了之后首先得设置个人信息，因为是分布式的系统，所以交互的时候要自报家门
```bash
$ git config --global user.name "Your Name"  
$ git config --global user.email "email@example.com"  
```  

##1	新建一个仓库，并且该仓库应该是git类型  

	1.1 git init新建git类型的仓库

##2	新建文件并放入git仓库    

	
	2.1	touch a.txt   touch a.txt或者vi  
	2.2	git add 将新建的a.txt纳入git管理  
	2.3	git status,查看文件在git仓库中的状态
	2.4	git commit -m "提交修改的信息说明",完成了首次提交
	2.5	新增文件内容，再次尝试提交。```

##3	git的日志和跟踪管理 
 
	
```bash
    3.1	git log,查看每次操作的日志情况。
		git log --pretty=oneline可以一行显示，查看关键信息
	3.2	git diff,查看内容不同。
	3.3 git checkout -- file 放弃工作区修改，其实是用版本库里的版本替换                         
	    工作区的版本。 
	3.4 git reset HEAD file  放弃暂存区修改，再进行3.3
	3.5 rm file,git rm file;   删除文件，能用checkout恢复
```

##4	git版本的回退  

	4.1	退一步，git reset --hard HEAD^，指针回退一步；
	4.2	退多步V1，git reset --hard HEAD^^^^^^^^^^,多个箭号
	4.3	退多步V2，git reset --hard HEAD~数字步数
	4.4	穿梭穿越，git reflog获得头7位版本号，然后
			git reset --hard 7位版本号

##5	git三区  
    
```bash
5.1 Working directory        工作区
5.2 Repository               版本区
5.3 Stage（index）           暂存区
```

##5  git远程仓库  

###第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
```bash
$ ssh-keygen -t rsa -C "youremail@example.com"
```  
用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。  
###第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容。  
###第3步：创建库，与本地关联、根据GitHub的提示，在本地的learngit仓库下运行命令：
```bash
$ git remote add origin git@github.com:michaelliao/learngit.git
```
###第4步，就可以把本地库的所有内容推送到远程库上：
```bash
$ git push -u origin master
```
把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程.由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
```bash
$ git push origin master
```
```bash
要关联一个远程库，使用命令
git remote add origin git@server-name:path/repo-name.git；

关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；
```

###第5步  
要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
```bash
git clone
```	
Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。
###第6步  
git remote查看远程库信息
###第7步
```bash
如果出现push错误，可以push -f强推
也可以pull，但是要先：
    $ git config branch.master.remote origin 
    $ git config branch.master.merge refs/heads/master 
```


##6	git分支  

	6.1	git branch 查看分支
	6.2	git branch 分支名字  新建分支
	6.3	git checkout 分支名  切换分支
	6.4	git merge dev		在master分支上合并dev分之内容
	6.5	git branch -d 分支名	删除分支
	6.6	git checkout -b 分支名	新建+切换一步搞定
	6.7 git merge --no-ff -m "merge with no-ff" dev
	6.8 $ git stash  一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；另一种方式是用git stash pop，恢复的同时把stash内容也删了：$ git stash list查看
	6.8 git branch -D feature-vulcan删除没有merge的分区
	6.8 git remote查看远程库信息
 
##7	第一种冲突  

	分支合并后的冲突，如何解决见VCR。
	
	第二种冲突
	push提交后的内容冲突，请先pull到本地人工干预收工合并后再push

	第三种冲突TortoiseGit和TortoiseSVN是同样的操作解决
	TortoiseGit--黄色三角感叹号---edit conflict---merge---resolve--commit--->OK

	第四种冲突，Egit处理
	有冲突了先pull，具体见Vcr


##8	TortoiseGit  

		

##9	EGit   
    

    [branch "master"]
    remote = origin
    merge = refs/heads/master

	[remote "origin"]
    url = your httpsaddress
    fetch = +refs/heads/*:refs/remotes/origin/*
    push = refs/heads/master:refs/heads/master




中午吃饭的时候,运行一下服务器本地的对比。很费时间，但是很有效。不要盲目的pull