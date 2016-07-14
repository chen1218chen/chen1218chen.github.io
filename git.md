---
title: git入门及常用命令
tags: git
grammar_cjkRuby: true
---

[TOC]
 
## 专有名词
- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库，指托管在网络上的项目仓库

.gitignore文件中列出要忽略的文件模式，例如：.class，忽略.class文件

![image1](./images/1450160316466.png)

## 本地项目上传github
```
 //设置用户
 git config --global user.name
 git config --global user.email
 cd d:
 mkdir bootstrap
 cd bootstrap 
 
 git init   //初始化版本仓库
 touch READM   //新建readme文件
 echo "hello world!" >>README
 
 git add . //将所有文件添加暂存区域
 git reset file //取消已暂存文件
 git commit -m 'first commit'  //提交缓存，并添加描述‘first commit’
 // 添加名字为origin的远程库，注意需提前建立好版本仓库,再次修改后提交跳过此命令
 git remote add origin https://github.com/chen1218chen/bootstrap.git

 git push origin master   //提交到github仓库
 git push //提交到local
 git pull //抓取远程数据并自动合并
 git fetch //抓取远程数据，需要手动合并
 
 git clone <url> //自动将远程库归于origin下

    
 git status  //查看提交状态
 git status -s   //更为紧凑的格式输出
 git status --short
 git log -p -2//-p显示每次提交的内容差异,-2最近两次更新

 git tag  //列出所有tag
 //新建一个tag在当前commit
 git tag [tag]
 //新建一个tag在指定commit
 git tag [tag] [commit]
 //查看tag信息
 git show [tag]
 //提交所有tag
$ git push [remote] --tags

 // 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
```
## 命令分析
 
### git help <verb>

### git mv 
修改文件名：

    git mv file_from file_to
    
### git rm
删除文件

    rm <file>
    git rm <file>
### git commit 
 
 git commit 提交时强制要求输入提交说明，不能省略，使用-m参数输入提交说明，如果没有，git会自动打开一个编辑器，要求在其中输入提交说明。
**参数说明：**
    -m 输入提交说明
    **--amend** 对刚刚的提交进行修补
    --allow-empty 空白被允许提交
    --reset-author 将author的ID同步修改
### git clone


    # 最简单的命令
    git clone xxx.git

    # clone到指定目录
    git clone xxx.git "指定目录"

    # clone时创建新的分支替代默认Origin HEAD（master）
    git clone -b [new_branch_name]  xxx.git
   
### git push
推送数据到远程仓库

    git push [remote-name] [branch-name]

### git remote 

git默认使用origin作为远程库名。master为默认主分支名，自动建立的，版本库初始化以后，默认在master主分支进行保存。

    git remote //查看远程库名
    git remote -v //查看远程库地址
    git remte show [remote-name] //查看远程仓库的详细信息
    git remote rename aa cc //修改远程仓库aa的名称
    git remote rm cc  //删除远程仓库cc


删除远程仓库报错，如下图所示：

 ![enter description here][1] 

原因是用法错误，`git remote rm <主机名>`用来删除添加的remote路径，正确用法如下图所示：

![enter description here][2]

常用命令：
![enter description here][3]
### git config

    git config --list //查看配置信息
    git config -e  //版本库级配置文件
    git config -e --global   //全局配置文件
    git config -e --system  //系统级配置文件
    git config --global core.editor
    git config --global core.ui true //为终端的内容着色


#### user.name、user.email
删除全局配置中的user.name和user.email
```
git config --unset --global user.name
git config --unset --global user.email
```
这样一来，查看
```
git config user.name
git config user.email
```
将看不到输出。

### git branch
![enter description here][4]
    
### git reset
![enter description here][5]

    git reset HEAD <file> //HEAD指针指向当前分支的最新的提交
    git reflog  //查看所有日志包括撤销的操作
    git reset --hard 2d87c9b  //回复到2d87c9b这个版本
    git reset HEAD^ 
    git reset HEAD^^
    git reset HEAD~1
    git reset HEAD~n
### git merge
查看支持的工具

    git mergetool --tool-help
![enter description here][6]
如果工具有效，设置merge.tool

    git config --global merge.tool p4merge
设置路径

    git config --global mergetool.p4merge.path c:/Users/my-login/AppData/Local/Perforce/p4merge.exe
## 常见问题
### error:failed to push som refs to
```
error:failed to push som refs to ......
```
解决方法：
 1、先输入 git pull origin master ，先把远程服务器github上面的文件拉下来
 2、再输入git push origin master
### fatal: remote origin already exists.
```
fatal: remote origin already exists.
```
解决方法：
   1、先输入 git remote rm origin
    2、再输入git remote add origin https://github.com/chen1218chen/bootstrap.git 就不会报错了！
### fatal: The remote end hung up unexpectedly
```
fatal: The remote end hung up unexpectedly
```
The problem is due to git/https buffer settings.
解决方法:
```
git config http.postBuffer 524288000
```

获取项目
```
git clone https://github.com/chne1218chen/bootstrap.git
```
### 更新时与本地文件冲突
报错：

    error: Your local changes to the following files would be overwritten by merge:
            _config.yml
            layout/_partial/footer.ejs
    Please, commit your changes or stash them before you can merge.
    Aborting
解决方法：

    # 暂存
    git stash
    git pull
    # 恢复
    git stash pop
    # 查看暂存
    git stash list
    # 清空暂存
    git stash clear


  [1]: ./images/Image%201.png "Image 1.png"
  [2]: ./images/Image%202.png "Image 2.png"
  [3]: ./images/1450160519441.png "1450160519441.png"
  [4]: ./images/1450160447699.png "1450160447699.png"
  [5]: ./images/1450160697037.png "1450160697037.png"
  [6]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20160530161258.png "微信截图_20160530161258.png"