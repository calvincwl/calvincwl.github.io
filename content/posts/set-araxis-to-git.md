---
title: 修改Git默认合并比较工具
date: 2017-06-06
lastmod: 2017-11-22
tags: 
- git
- github
categories:
- Git
---

刚开始使用GitHub，感觉做比较之类的工具kdiff3比较粗糙，想要使用Araxis的工具来可视化Merge/Diff，于是上网找了点方法。
***
# 设置Merge工具

1. 指定Merge工具名
		git config --global merge.tool araxis

2. 指定Merge工具路径
		git config --global mergetool.araxis.path 'C:\Program Files\Araxis\Araxis Merge\Compare'

# 设置Diff工具

1. 指定Diff工具名
		git config --global diff.tool araxis

2. 指定Diff工具路径
		git config --global difftool.araxis.path 'C:\Program Files\Araxis\Araxis Merge\Compare'

# 说明
Araxis现在使用Compare工具就可以实现Merge和Diff的功能，比起单独配置AraxisGitMerge和AraxisGitDiff工具更方便（无需设置各种参数）。