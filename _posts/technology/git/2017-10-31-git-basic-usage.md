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
```shell
git clone git://vt-sync/vmm_tree.git
```

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

### 7.切换commit号
```shell
git reset --hard commit-number
git checkout commit-number     #这个命令可以达到目的，不过一般用reset,checkout一般用于切换分支
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

### 10.git revert撤回 ###

~~~shell
# 撤销前一次 commit
git revert HEAD                  

# 撤销前前一次 commit
git revert HEAD^

# 撤回指定commit-id
git revert 0818badf6882ea2664a205bc8ef3a85425bb2537

#注意：1.revert撤销某次操作，此次操作之前和之后的commit和history都会保留。2.revert是撤回指定版本的内容并把这次撤销作为一个新的commit，不影响之前提交的内容，因此要重新生成一个patch,并做push操作。
~~~

**如果要把某次操作之前的commit全部撤回，用git reset,此时不用生成patch,直接push --force**

~~~shell
sles12-huangj43-dev-00:/home/hjl21.github.io # git reset --hard b4017001e2a5d66377dcde7ff53883bfbdafc390
HEAD is now at b401700 slightly modified style
sles12-huangj43-dev-00:/home/hjl21.github.io # git push origin master:master --force
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: GitHub found 2 vulnerabilities on hjl21/hjl21.github.io's default branch (2 moderate). To find out more, visit:
remote:      https://github.com/hjl21/hjl21.github.io/network/alerts
remote:
To https://github.com/hjl21/hjl21.github.io.git
 + 67ebb72...b401700 master -> master (forced update)
sles12-huangj43-dev-00:/home/hjl21.github.io # 
~~~

git revert 和 git reset的区别: 
1. git revert是用一次新的commit来回滚之前的commit，git reset是直接删除指定的commit。 
2. 在回滚这一操作上看，效果差不多。但是在日后继续merge以前的老版本时有区别。因为git  revert是用一次逆向的commit“中和”之前的提交，因此日后合并老的branch时，导致这部分改变不会再次出现，但是git  reset是之间把某些commit在某个branch上删除，因而和老的branch再次merge时，这些被回滚的commit应该还会被引入。 
3. git reset 是把HEAD向后移动了一下，而git revert是HEAD继续前进，只是新的commit的内容和要revert的内容正好相反，能够抵消要被revert的内容。

