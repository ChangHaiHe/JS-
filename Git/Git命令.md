
# Git常用命令
 ![Git常用命令](https://upload-images.jianshu.io/upload_images/3985563-c7f05348b711ebbe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

## 本地分支管理

* git branch 查看当前本地有多少分支
* git branch -v 查看每个本地分支的`最后一次提交`
* git branch -vv 查看本地分支的`所有跟踪分支`
* git branch branch-name 创建分支(创建本地分支)
* git branch -d branch-name 删除branch-name分支(删除本地分支)
* git branch --merged 查看合并到当前分支的分支有哪些
* git branch' --no-merged 查看未合并到当前分支的分支有哪些


## 远程分支管理

* git push origin --delete branch-name 删除远程分支
* git branch -d branch-name 删除本地分支

## 分支切换

* git checkout branch-name 切换分支
* git checkout -b branch-name 创建分支并切换到新分支上, 同时，如果远程有同样的分支名，自动跟踪远程分支

  > eg: git checkout -b e
    在本地分支e 上开发完事, add 和 commit之后直接 git push origin e


## 常用

  * git branch 列出`所有本地分支`
  * git branch -r 列出`所有远程分支`
  * git branch -a 列出`所有本地`分支和`远程`分支
  * git log 显示当前分支的版本历史
  * git diff 显示暂存区和工作区的差异 (在"git add ."之前"git diff")
  * git diff HEAD 显示工作区与当前分支最新commit之间的差异

## 回滚

  * git reset --hard HEAD 让工作区回到上次提交时的状态
  
  > git log 之后看哈希值, 比如: commit 95ec9f57e0a1d9b4c03a6345d4859130bd499337
    需要回到这个版本, 就git reset --hard 95ec -> success

  [git reset revert 回退回滚取消提交返回上一版本](http://yijiebuyi.com/blog/8f985d539566d0bf3b804df6be4e0c90.html)
 

  * git log --pretty=oneline 查看所有commit提交的记录以及哈希值
  * git reset --hard HEAD^ 回滚到上一个版本
    git reset --hard HEAD^^ 回滚到上上个版本(往上100个版本写100个^, 所以写成HEAD~100)
    git reset --hard HEAD~3 回滚到前三次提交的位置

## 其他相关
* git --version 查看当前使用的git版本
* git log --oneline --decorate --graph --all 查看提交历史, 分支指向以及项目分支分叉情况
* git merge branch-name 将branch-name合并到当前分支上

  > 比如，现在总共有 master, a, b, c三个分支，
    此时正处于b分支上:
      git merge master则表示将master分支合并到b分支上
    此时正处在master分支上:
      git merge b则表示将b分支合并到master分支上
* git push [remote] [branch] 推送分支到远程仓库

  > git push origin master 将本地分支推送到远程master分支上(前提是本地是在master分支下push的)
    git push origin a 将本地分支推送到远程a分支上(前提是本地是在a分支下push的)

---------------------------------------
## 开发常用的步骤

  git status // 查看当前状态
  git add . // 添加到暂存区
  git commit -m "提交数据" // 提交到仓库
  git push origin master (如果是在其他分支上则是 git push origin 相应分支名) // 推送到线上远程分支
  
  git pull //拉取代码
  git merge 分支名 //合并当前分支

  git log 查看本地提交记录
  git reset --hard HEAD^ 回滚到上一个(commit)版本


## 合并分支
比如现在自己在分支daily/1.4.12上开发

  git checkout master
  git pull
  git checkout daily/1.4.12
  git merge master
  git push origin daily/1.4.12

## 发布项目

发布之前需要合并一下主分支
  git checkout master
  git pull
  git checkout daily/1.4.12
  git merge master
  git tag publish/1.4.12
  git push -u origin publish/1.4.12

## 发布npm包
比如有一个uxcore-button的包开发完成需要发布
  git pull
  npm run build
  "version": "0.1.32" 修改package.json里的版本号
  npm publish 发布(发布之前必须npm run build)

  git add .
  git commit -m "发布新版本"
  git push origin master
注意: 其他项目引用到uxcore-button这个包的地方都需要修改相应新发布的版本号"0.1.32"




  
