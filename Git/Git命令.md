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
* git branch -a 查看所有远程分支

## 分支切换
* git branch 查看当前所在的分支
* git checkout branch-name 切换分支
* git checkout -b branch-name 创建分支并切换到新分支上, 同时，如果远程有同样的分支名，自动跟踪远程分支
  > eg: git checkout -b e
    在本地分支e 上开发完事, add 和 commit之后直接 git push origin e



## 其他相关

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
