## git clone 其它分支

首先, clone 一个远端仓库，到其目录下：

	$ git clone git://example.com/myproject $ cd myproject

然后，看看你本地有什么分支：

	$ git branch * master

但是有些其他分支你在的仓库里面是隐藏的，你可以加上－a选项来查看它们：

	$ git branch -a
	* master
	remotes/origin/HEAD -> origin/master
	remotes/origin/dev
	remotes/origin/master

如果你现快速的代上面的分支，你可以直接切换到那个分支：

	$ git checkout origin/experimental

但是，如果你想在那个分支工作的话，你就需要创建一个本地分支：

	$ git checkout -b experimental origin/experimental

现在，如果你看看你的本地分支，你会看到：

	$ git branch
	  master
	* experimental

你还可以用git remote命令跟踪多个远程分支