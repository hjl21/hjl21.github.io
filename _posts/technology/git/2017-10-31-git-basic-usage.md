---
layout: post
title: git基本用法
date: 2017-10-31 14:21:00 +0800
categories: 技术文档
tag: Git
---

* content
{:toc}

# Git basic usage

### 1.git clone(下载code)
	git clone git://vt-sync/vmm_tree.git

### 2.Git configure
```shell
git config --global user.name "Huang Junliang"
git config --global user.email junliangx.huang@intel.com

.gitconfig(/root/.gitconfig)
```

### 3.创建分支
	基于远程分支”origin”，创建一个叫”mywork”的分支。
	git checkout -b mywork origin

### 4.打patch步骤
```shell
1.git status
2.git add
3.git commit -a
4.git format-patch -1 -o +路径[1指patch的个数]
```

### 5.修改patch
```shell
a. 合并上次提交和本次提交的
    git commit -a(将 untagged 变成 tagged)
b.  修改代码
    git commit --amend -a  
c.  如果不修改代码，只是修改提交的注释的话
    git commit --amend
d. 合并中间有间隔的几次提交
    git rebase -i HEAD~3
    修改 pick 的顺序  
e. 提交并增加签名
    git commit -a -s
f. 提交并添加注释 
    这是 git 基本工作流程的第一步；使用如下命令以实际提交改动：
    git commit -m "代码提交信息"
    现在，你的改动已经提交到了 HEAD，但是还没到你的远端仓库。
```

### 6.合并patch
```shell
git rebase -i HEAD~2(合并2个patch)
[pick 保留，squash合并到保留选项]
git branch -a         ---查看所有分支
git checkout + 分支名  ---切换分支

git tag
git remote -v         ----查看远端服务器上的仓库路径
```

### 7.返回到某个patch
```shell
git reset --hard commit-number
```

### 8.Pull patch
```shell
git reset --hard commit 号                 撤销所打的patch
git show commit 号                        查看所提交的patch内容
git commit -am "add a dd script(patch的名字)"
git am -3 -i -s -u <patch>
git push origin cicada-test:cicada-test
```

### 9.merge patch 
```shell
git push origin YOURNAME_FEATURE:YOURNAME_FEATURE
git checkout cicada-test
git merge YOURNAME_FEATURE
```

