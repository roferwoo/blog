---
title: Git Stash用法
date: 2017-04-27 16:55:33
categories: [Git]
description: Git,Stash,Git命令,MacBook Pro,Mac,Tools,Mac开发常用工具
---
在编码过程中经常会碰到突发BUG，需要紧急修复的场景（*BUG高于需求*）。可能与你写到一半的代码（*没有完成，不该提交*）产生冲突，这时Git为我们提供`git stash`命令将工作现场**暂存**起来，等以后恢复现场后继续之前未完成的编码。相关命令参考：

> 注：[]方括号中内容为可选，[<stash>]里面的stash代表进度的编号形如：stash@{0}, <>尖括号内的必填。

- `git stash`
对当前的暂存区和工作区状态进行保存。
- `git stash list`
列出所有保存的进度列表。
- `git stash pop [--index] [<stash>]`
恢复工作进度现场。
`--index`参数表示不仅恢复工作区，还恢复暂存区；
`<stash>`指定定恢复某个具体进度，如果没有这个参数，则默认恢复最新的进度。如：`git stash pop --index stash@{0}`表示恢复编号为0的进度的工作区和暂存区。
- `git stash [save message] [-k|--no-keep-index] [--patch]`
**这是`git stash`最完整的命令形式**。使用`save`可以对进度添加备注，如`git stash save "备注"`。
`-k`和`--no-keep-index`指定保存进度后，是否重置暂存区。
`--patch`会显示工作区和HEAD的差异，通过编辑差异文件，排除不需要保存的内容。
- `git stash apply [--index] [<stash>]`
不删除已恢复的进度，其他同`git stash pop`。
- `git stash drop [<stash>]`
删除某一个进度，默认删除最新进度。
- `git stash clear`
删除所有进度。
- `git stash branch <branchname> <stash>`
基于进度创建分支。