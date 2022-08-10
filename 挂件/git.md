# Git

## 基本命令

```
git init//初始化本地库
git add 文件名//添加到暂存区
git commit -m"注释"//提交版本库
git status//查看本地库状态
git remote add origin git@gitee.com:how-to-learn/仓库名称.git//首次关联远程仓库
git push -u origin master//首次推送远程仓库
git push 分支==>推送远程仓库
```

## 版本操作命令

```
 git log，git log --pretty=oneline==>查看版本历史(前者详细版，后者精简版)
 git reflog==>查看命令历史(近似于查看版本历史的精简版，但它还收录了命令，还有可以查看去到未来的版本号)
 git reset --hard commit_id(版本号)==>版本穿梭，可以去到历史的任一时期
```

## 分支命令

```
git branch 分支名//创建分支
git branch -v//查看分支
git checkout 分支名//切换分支
git merge 分支名//合并分支，把指定的分支合并到当前分支上
git branch -d 分支名//删除分支
```

## 远程仓库操作命令

```
git config --global user.name==>设置用户签名，用户名(必须设置，否则提交时会报错)
git config --global user.email==>设置用户签名，邮箱(必须设置，否则提交时会报错)
git remote -v==>查看远程库信息
git remote rm 远程仓库名==>删除和远程库的关联
git remote add 别名 远程地址==>创建别名
git clone 远程地址==>将远程仓库的内容克隆到本地(远程拉取项目后第一时间要npm i 安装所有依赖)
git pull 远程库地址别名 远程分支名==>将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并
git push==>推送至远程版本库
git pull==>拉回远程版本库的提交
```

## 删除命令

```
git checkout -- 文件名==>撤销修改，把工作区的修改撤销
git reset HEAD 文件名==>撤销修改，把暂存区的修改撤销
git reset --hard HEAD^==>撤销修改,回退到上一个版本
git rm 文件名==>删除文件
```

## 未归类命令

```
git --version==>查看git版本
git diff HEAD -- 文件名==>查看工作区和暂存区的区别
git ls-files --stage==>查看暂存区的内容
mkdir 目录名==>创建目录
cd 目录名==>进入目录
pwd==>显示当前目录
cat 文件名.后缀名==>查看文件
vim 文件名.后缀名==>查看、编辑文件
Esc+yy==>复制
p==>粘贴
:wq!==>保存
```

## 注解内容

Git的快照被称为commit,HEAD是当前版本,上一个版本是HEAD^(以此类推)，也可以HEAD~1

git本地库:工作区+暂存区+版本库

工作区：在电脑里能看到的项目目录(不包括.git)，所有本地的改动都是在工作区改动的，工作区是对项目的某个版本独立提取出来的内容，是从git仓库的压缩数据库中提取出来的文件，放在磁盘上以供使用或修改

暂存区：暂存区是一个存在于.git的index文件(.git/index),保存了下次将要提交的文件列表信息

版本库：版本库就是.git，暂存区被包含在版本库中。其中.git中的objects目录是对象数据库

分支合并冲突：合并分支时，如果两个分支在同一个文件的同一个位置有两套完全不同的修改，将会产生分支合并冲突，Git无法替我们决定使用哪一个，必须人为决定新代码内容(vim 文件查看文件，然后i开始编辑，之后Esc取消编辑状态，:wq!退出并保存),人工合并后，只会在当前分支上进行合并，指定分支内容不变

master、hot-fix其实都是指向具体版本记录的指针，当前所在的分支，其实是由HEAD决定的，所以创建分支的本质就是多创建一个指针

HEAD指向哪个分支，我们就在哪个分支上--切换分支的本质就是移动HEAD指针

https链接：https://github.com/QSBQ/experiment.git

SSH链接：git@github.com:QSBQ/experiment.git

## 出错时的解决方案

```
git推送10053失败解决方案:git config --global http.sslVerify false
443 after 21073 ms解决方案:git config --global --unset http.proxy
长双错误解决方案:git config --global http.sslVerify true
```

## github

```
git remote add origin https://github.com/QSBQ/emperor.git
```

## git相关问题

```
pip install requests//使用git拉取python代码时，经常需要下载requests才能使用
```

```
短信轰炸命令：pipenv run python smsboom.py run -t 64 -p 17876442627 -s -i 5
我的github访问令牌：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
```