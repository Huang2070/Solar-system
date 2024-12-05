command + ~，该快捷键可以实现同程序不同窗口之间的循环切换.

***

shift+command+., 显示隐藏文件

***

在Finder顶部显示文件夹路径

命令: defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES
killall Finder

复制路径可以在当前文件夹下使用快捷键:Option+Command+C.

*** 

如果想在Finder中就能直观看到隐藏文件，那么在终端中输入以下下命令：

1. defaults write com.apple.Finder AppleShowAllFiles YES    
2. killall Finder.

要关闭显示隐藏文件的话就把上面的命令中YES改为NO就行了

*** 



