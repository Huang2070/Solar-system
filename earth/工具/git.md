# 本地建立项目推送到github

## 1.先在本地建立git库
1. 新建项目目录,cd到目录下
2. 新建readme文件: touch README.md
3. 初始化git: git init
4. 查看git设置的邮箱是否和github一致: git config user.email, 如果不是想要的邮箱,则修改git邮箱: git config user.email '邮箱地址'.
5. 注意!现在github的主分支都叫main, git config --global init.defaultBranch main, 可以全局修改默认的init时的主分支. git branch -M '分支名', 可以修改当前分支名.
6. git add .
7. git commit -am "first commit".

## 2.在github上建立远程库

## 3.推送本地代码到远程库
1. git remote add origin https://github.com/Huang2070/***.git
2. git push -u origin main


## 其它
* 如果push失败,先pull下代码,再push
* 强制pull代码命令：git pull origin main --allow-unrelated-histories
* git现在必须使用token提交代码, 用下面命令设置远程地址为如下格式, 即可以不输入密码push, 如果提交时要输入密码, 这时输入token即可.git remote set-url origin https://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@github.com/Huang2070/alpha-centauri-c.git



# 恢复历史版本

1. 查看所有版本号,从上到下为从新到旧:git log --pretty=oneline
2. 查看某版本修改的内容: git show codeVersion
3. 恢复到某版本: git reset --hard codeVersion
4. 提交到远程分支: git push -f

## 几种reset模式
1. soft
移动本地库HEAD指针
意思就是，回滚后，仅仅是把本地库的指针移动了，而暂存区和你本地的代码是没有做任何改变的。而你上次改动已提交committed到本地库的代码显示是绿色即未提交

2. mixed
①移动本地库HEAD指针
②重置暂存区
意思就是，回滚后，不仅移动了本地库的指针，同时暂存区的东西也没了，意思就是你上次添加到暂存区的文件没了

3. hard
①移动本地库HEAD指针
②重置暂存区
③重置工作区
意思就是，回滚后，本地代码就是你回退版本的代码

4. keep
①移动本地库HEAD指针
②暂存区不变
③重置工作区
意思就是，回滚后，本地代码就是你回退版本的代码,而暂存区是没有做任何改变的
