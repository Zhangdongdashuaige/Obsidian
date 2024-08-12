[toc] 
# 指令
	git add 文件名 将指定文件添加至缓冲区
	git commit 将缓冲区文件提交至本地仓库
	git init 在当前目录初始化仓库
	git restore 从本地库中恢复文件
	git reset 恢复指定版本，不保留最新记录
	git revert 恢复指定版本，保留最新记录
	git branch 新建分支，分支操作是基于提交的，如果没有提交就没有分支
	git branch -v 查看分支
	git checkout 分支名 切换至指定分支
	git checkout -b order 创建order分支并切换至order分支
	git branch -d user 删除user分支
	git merge 分支名 将指定分支合并至当前分支
		若有文件冲突，则修改冲突的文件后在执行GIT ADD | GIT COMMIT即可
	git tag tagname versionid 给指定版本号的提交增加标签
	git remote add origin url 指定远程仓库
	git remote rename origin new_name 改名
	git push 推送至远程仓库
	ssh-keygen -t rsa -C ssh地址 为ssh地址提供安全认证  
