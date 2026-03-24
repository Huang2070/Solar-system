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



# 其它
* 删除所有本地分支: git branch | xargs git branch -d