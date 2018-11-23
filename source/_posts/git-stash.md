---
title: git stash
date: 2018-11-23 15:02:44
tags:
  - git
---
##### 适用场景：
&nbsp;&nbsp;&nbsp;&nbsp;在当前分支开发，此时其他分支出现bug需要马上修改，当前分支的代码未完成，不想提交到远程仓库时，可以使用 `git stash` 将当前的工作区缓存起来，然后切换到修复bug的分支，当修复完bug后，再切回原来的分支，使用 `git stash pop` 将工作区恢复。

注意：stash 不区分分支

#### 用法：
1. `git stash list` : 查看所有 stash
2. `git stash` : 保存一条 stash
3. `git stash save "[message]"` : 保存一条stash，并给此stash添加描述信息（`[]`内为描述信息）
4. `git stash pop` : 检出最新的一条stash，并将其从 stash list 中清除
5. `git stash apply stash@{[num]}` : 检出指定的一条 stash（`[]`内为 stash 的标志数字，表示第几条 stash）
6. `git stash drop stash@{[num]}` : 删除指定的一条 stash（`[]`内为 stash 的标志数字，表示第几条 stash）