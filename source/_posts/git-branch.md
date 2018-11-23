---
title: git branch
date: 2018-11-23 15:24:41
tags:
 - git
---
1. `git branch -a`  列出所有分支，包括本地和远程分支
2. `git branch`  列出本地分支
3. `git branch [newBranchName]`  创建新分支
4. `git checkout -b [newBranchName]` 创建并切换到新分支
5. `git checkout [branchName]`  切换到某个分支
6. `git branch -d [branchName]`  删除分支
7. `git push origin [branchName]`  将本地分支推向远程
8. `git push origin :[branchName]`  删除远程分支
<!-- more -->
9.  `git push origin [localBranchName]:[remoteBranchName]`  若 remoteBranchName 不存在，则在远程自动创建分支；若 localBranchName 不存在，则删除远程分支。localBranchName  为本地存在的分支，remoteBranchName 为远程分支
10. `git merge [branchName]`  将分支合并到当前分支
11. `git branch -m [oldBranchName] [newBranchName]`  本地分支重命名
12. 远程分支重命名：
  1.  `git branch -m [oldBranchName] [newBranchName]`  首先本地分支重命名
  2.  `git push origin :[oldBranchName]`  删除远程分支
  3.  `git push origin [newBranchName]:[newBranchName]`  将本地新命名分支推向远程
13. `git branch -vv`  本地分支和服务器分支的映射关系
14. `git reflog --date=local | grep [branchName]`  查看分支的父分支
15. `git branch --set-upstream-to=origin/[remoteBranchName] [localBranchName]`  将本地分支和远程分支关联起来
