---
title: 使用git进行版本控制
date: 2023-08-15 22:39:36
tags: python
---

# 使用git进行版本控制

四步：

> 进入文件夹
>
> 初始化：`git init`
>
> 管理
>
> 生成版本

## 常规管理命令

git的三大区域：

工作区(working directory)、暂存区(staging area)、版本库(repository)

`git status`:检测当前文件夹下的文件状态

> 红色：新增的文件，或新修改的文件
>
> 绿色：已经管理起来的文件，还未生成版本
>
> 未出现：已经生成版本后未有修改

`git add 文件名`：将某文件添加进管理列表

`git add .`：将此文件夹下所有文件添加进管理列表

`  git config --global user.email "you@example.com"`：配置个人邮箱
`git config --global user.name "Your Name"`：配置个人用户名

`git commit -m '版本名称'`：将此时管理列表中的文件生成一个版本

`git log`：查看生成版本记录，记录了版本号

`git reflog`：查看包含已回滚的版本记录，记录了版本号

`git reset --hard 版本号`：回滚到某个版本，回滚之后，当前版本记录将会消失

`git checkout -- 文件名`：回到文件未修改时的状态

`git reset HEAD 文件名`：从暂存区回到工作区

`ls`：展示文件列表

`git touch 文件名`：添加文件

` vim 文件名`：编辑文件

`cat 文件名`：查看文件内容

## 分支管理命令

git一般是在主分支(master)上控制版本，当需要开发新功能时，或遇到线上紧急bug需要修复时，可以拓展新的分支，在不影响开发新功能的情况下修复线上bug，待新功能开发完成且线上bug修复之后再进行合并进入主分支。

`git branch`：查看分支，绿色代表当前版本所在分支

`git branch 分支名`：创建新分支

`git checkout 分支名`：进入某分支

`git merge 分支名`：将某分支合并到主分支

> 需要先切换回主分支，然后使用这个命令合并其他分支
>
> 合并分支可能产生冲突，需要手动解决

`git branch -d 分支名`：删除某分支

## git hub代码托管

git hub是一个代码托管仓库，可以将git中的版本推送到git hub中，实现云端的版本控制。

使用git hub的流程

> 注册账号	用户名：dengjinjun	密码：Deng@3364546586

> 创建仓库

> 本地代码推送

```
git remote add origin 仓库url			//给远程仓库地址起别名，只需要操作一次
git push -u origin 分支名				//向远程仓库推送代码，-u代表默认分支
git push origin 分支名					//手动选择分支
```

> 将仓库中的代码克隆到本地

```
git clone 仓库url			//全部克隆，适用新环境第一次拉取代码
git pull origin 分支名		//更新代码
```

## re base(变基)

re base的作用是将繁琐的版本记录合并，更简洁地显示

### 某分支上的多个记录整合为一条记录

- 执行合并命令

```
git rebase -i 版本号		//将当前版本一直到所指定的版本所有版本合并为一个版本
```

```
git rebase -i HEAD~3		//将最近的三个版本合并为一个版本
```

- 选择合并方式

  ```
  s:合并到上一个版本
  :wq			//保存
  ```

- 选择版本名称

  ```
  输入版本信息
  :wq			//保存
  ```

> 注意：尽量不要合并已经提交到git hub上的版本

### 使某分支记录与主分支记录合并为一条记录

简洁显示提交记录

```
git log --gragh --pretty=format:"%h %s"
```

- 切换到某分支，执行变基指令，以主分支master为基

  ```
  git checkout dev
  git rebase master
  ```

- 切换到master分支，合并dev分支

  ```
  git checkout master
  git merge dev
  ```

> 选择合并而不变基会在版本日志中显示分支记录，详细而不简洁；变基操作不会再版本日志中显示分支记录，简洁但不够详细；选择哪种方式取决于具体需求。

### 本地存在未推送的代码，还需要从Git Hub拉取代码，使其不产生分支

```
git pull origin dev 指令可以分解为两条指令：
	git fetch origin dev 将云端代码拉到本地版本库
	git merge dev 合并
```

这种情况可以不使用`git pull`指令,而是

```
git fetch origin dev
git rebase origin/dev
```

### 汇总

![image-20230328003801459](assets/image-20230328003801459.png)

> 注意：使用`git rebase`可能会产生冲突而报错，`rebase`命令还未中断，此时可以手动解决冲突并暂存，然后执行`git rebase --continue`来完成变基。

## beyond compare快速解决冲突

- 安装beyond compare

- 在git中配置

  ```
  git config --global merge.tool bc4
  git config --global mergetool.bc4.cmd "\"D:\\bc4\\BComp.exe\" \"\$LOCAL\" \"\$REMOTE\" \"\$BASE\" \"\$MERGED\""
  git config --global mergetool.bc4.trustExitCode true
  git config --global mergetool.keepBackup false
  ```

- 使用软件解决冲突

  ```
  git mergetool
  ```

## 多人协同开发

给版本号添加标签和描述

```
git tag -a v1 -m "版本描述"		// 打上标签和描述，更加直观
git push origin --tags			// 推送到远程
```

- git flow工作流

  ![image-20230328153809944](assets/image-20230328153809944.png)

- 代码review

  创建分支，可通过网页review或拉取到本地review

- 代码release

  同代码review

- 给开源软件贡献代码

  > fork到自己的Git Hub仓库
  >
  > 拉取到本地进行修改
  >
  > 给源代码作者提交修复bug的申请（pull request）