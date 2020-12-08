# GIT
# 初始化Git仓库
- 这个仓库会存放，git对我们项目代码进行备份的文件
- 在项目目录右键打开 git bash
- 命令： ` git init `

# 在git中设置当前使用的用户是谁
- 命令：
    + 配置用户名： ` git config --global user.name "     "  `
    + 配置邮箱：` git config --global user.email  "   " `

# 把代码放到 .git仓库中
- 1 把代码放到仓库门口
    + ` git add ./readme.md ` 所指定的文件放到大门口
    + ` git add ./ ` 所有的修改文件放到大门口
  2 把仓库门口的代码放到里面的房间中去
    + ` git commin -m "说明" `
- 一次性把修改的代码放到里面的房间中去
    + ` git commit --all -m "说明" `

# 查看当前的状态
- 可以用来查看当前代码有没有被放到仓储中去
- 命令: `git status`

# git中的忽略文件
- .gitignore,在这个文件中可以设置要被忽略的文件或者目录。
- 被忽略的文件不会被提交仓储里去.
- 在.gitignore中可以书写要被忽略的文件的路径，以/开头，
    一行写一个路径，这些路径所对应的文件都会被忽略，
    不会被提交到仓储中
    + 写法
        * ` /.idea  ` 会忽略.idea文件
        * ` /js`      会忽略js目录里的所有文件
        * ` /js/*.js` 会忽略js目录下所有js文件

# 分支
- 默认是有一个主分支master

## 创建分支
- `git branch dev`
    + 创建了一个dev分支
    + 在刚创建时dev分支里的东西和master分支里的东西是一样的

## 切换分支
- `git checkout dev`
    + 切换到指定的分支,这里的切换到名为dev的分支
    `git branch` 可以查看当前有哪些分支


## 合并分支
- `git merge dev`
    + 合并分支内容,把当前分支与指定的分支(dev),进行合并
    + 当前分支指的是`git branch`命令输出的前面有*号的分支
- 合并时如果有冲突，需要手动去处理，处理后还需要再提交一次.

# GitHub
- 创建SSH KEY。先看一下你C盘用户目录下有没有.ssh目录，有的话看下里面有没有id_rsa和id_rsa.pub这两个文件，有就跳到下一步，没有就通过下面命令创建
　　 $ ssh-keygen -t rsa -C "youremail@example.com"
       然后一路回车。这时你就会在用户下的.ssh目录里找到id_rsa和id_rsa.pub这两个文件   
- 登录Github,找到右上角的用户图标，打开点进里面的Settings，再选中里面的SSH and GPG KEYS，点击右上角的New SSH key，然后Title里面随便填，再把刚才id_rsa.pub里面的内容复制到Title下面的Key内容框里面，最后点击Add SSH key，这样就完成了SSH Key的加密。
- 在Github上创建一个Git仓库。
    可以直接点New repository来创建
- 在Github上创建好Git仓库之后我们就可以和本地仓库进行关联了，根据创建好的Git仓库页面的提示，可以在本地test仓库的命令行输入：
 　 ` git remote add origin git@github.com:smfx1314/test2.git`(其中git@github.com:smfx1314/test2.git是code下面的http/ssh地址)
- 关联好之后我们就可以把本地库的所有内容推送到远程仓库（也就是Github）上了，通过：
　　 $ git push -u origin master
    如果新建的远程仓库是空的，所以要加上-u这个参数。

test is true