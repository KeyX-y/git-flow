


## 之前工作流
![e3eadf1a1313792088380cab1853ca8c.jpeg](evernotecid://A7C0564F-2F34-4804-B343-43BA95B5664D/appyinxiangcom/18111462/ENResource/p35)@w=1000



## 问题梳理
1. 功能分支依赖性较高
2. 没有给开发多功能分支测试环境
3. release分支多次合并，commit提交混乱
4. 线上版本和master不同步
5. 需专门人员负责release,master分支管理


## 当前流程

![80388c3e530ae84dfc067e332bcbdb8b.jpeg](evernotecid://A7C0564F-2F34-4804-B343-43BA95B5664D/appyinxiangcom/18111462/ENResource/p30)@w=800



**解决方案**

* master 用于同步线上版本，所有发到线上release分支，必须由相关人员合并到master,不允许直接修改master分支;
    _好处：_

    > master同步线上版本，有益于问题追述，线上产生问题不是去查看是哪个版本，而是直接看master代码
* release 用于测试人员发布测试、离线和生产环境，并约束release分支的合并

     _好处：_
   > release分支作为上线分支，应避免单个功能分支的多次合并，凡是合并到release分支的功能分支，有bug需从release分支切出分支修改，再合并到release上。有利于多人协作commit提交干净的历史记录


* dev 用于功能开发，开发人员将自己开发功能分支合并到dev分支，用于dev环境的部署，功能测试
  _好处：_
  > 新建dev环境，有利于开发人员，测试多个功能分支
* feature 功能分支，由开发人员自行创建，进行功能开发
 _好处：_
     > 以功能为基础，专注功能开发，没依赖


## git flow


![7e8ae9816f59a548013b38fdb4566cb5.jpeg](evernotecid://A7C0564F-2F34-4804-B343-43BA95B5664D/appyinxiangcom/18111462/ENResource/p3)@w=800


a. 创建feature分支

```
保证分支从master创建出来（本地master需与远端同步）
git checkout branch feature-a
```

b. 提交修改到暂存区

```
 git add some-file 或者 git add . 
```

c. 提交暂存区到本地仓

```
git commit -m 'some describe'
```

d. 提交本地分支到远端

```
git push origin -u feature-a:feature-a
```
e. 合并当前分支到dev环境 （用于开发人员，检测功能）

```
git checkout dev
git merge feature-a
git push
```

f.在gitlab上提交MR以便相关人员进行code review;

g.合并当前分支到release分支，用于测试，离线，线上版本发布
```
git checkout release
git merge feature-a
git push
```

h. **由相关人员合并release分支到master,并打tag(tag号根据产品版本号来定)**



##  分支操作规则

1. 分支新建
>所有功能分支从master切换出来，并以feature开头;
>所有bug分支从master切换出来，并以hotfix开头;
>所有发离线，线上release分支从master切换出来，以release开头;

2. 分支合并
>功能，bug分支合并到dev,用于开发，测试；
>release, master分支的合并由相关人员负责

3. 分支删除
>所有上线后的分支由相关人员删除，

4. 分支操作
>分支的操作（新建，合并，删除）由本地来完成，并与远端同步

5. release分支操作规则
>所有功能分支合并到release后不允许再次合并，如需调整，必须从release分支上新切分支；
>release分支由相关人员负责；

## 建议

1. 所有git操作在本地执行，远端只做同步；
2. 采用命令行和sourcetree结合方式，进行git操作;
3. 分支合并，使用rebase 和 merge 结合；
4. commit编写详细，以便有问题版本会退；


## GitFlow工作流常用操作流程

![a2f402def4d239cb73039d63ea2b0c01.png](evernotecid://A7C0564F-2F34-4804-B343-43BA95B5664D/appyinxiangcom/18111462/ENResource/p54)@w=500



## 常用git技巧
1.当前commit和上次提交commit合并 (只能在自己功能分支操作)
```
git commit --amend"
如果远端有当前分支，需:git push -f
```

2.进行commit相关操作（合并，删除等）(只能在自己功能分支操作)
```
git rebase -i commit-info
如果远端有当前分支，需:git push -f
```

3.克隆其他分支commit

```
git cherry-pick commit-info
```

4.版本会退
```
git reset --hard HEAD~1 回退到工作区
git reset --soft HEAD~1 回退到暂存区
```
5.缓存当前做，回头再做
 ```
 git add . 提交工作区内容
 git stash 缓存正在做的
 git stash （pop | apply）pop 提取缓存内容，并(删除 | 保留)
 git stash pop drop 删除缓存
 ```
 
6.查看暂存区，仓库做的更改
```
git diff (--staged | --cached)
```

7.dev分支重建
```
git branch -D dev;
gco dev
git push origin :dev
ggpush -u
git checkout dev && git fetch && git reset --hard origin/dev
```

8.强制远程master覆盖本地master
```
git fetch --all
git reset --hard origin/master
```
 
 


