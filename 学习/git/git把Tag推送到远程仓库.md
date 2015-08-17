## Git把Tag推送到远程仓库

git tag可用于给工程打上标签，但是这个命令只是在本地仓库打标签。
	
	git tag "v2.0"

默认情况下，git push并不会把tag标签传送到远端服务器上，只有通过显式命令才能分享标签到远端仓库。

1.push单个tag，命令格式为：git push origin [tagname]
	
	git push origin v1.0 #将本地v1.0的tag推送到远端服务器

2.push所有tag，命令格式为：git push [origin] --tags

	git push --tags

	git push origin --tags

成功推送标签之后，仓库中 release 会出现tag标记的版本。

如果不起作用，请在Git控制台上确认你的账号是否有权限推送Tag。