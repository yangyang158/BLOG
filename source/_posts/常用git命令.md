---
title: 常用git命令
date: 2019-01-31 13:44:14
tags:
---
## 合并代码
    合并某次提交： git cherry-pick commitid
    合并某个分支： git merge 分支

## 操作分支
    删除远程分支：git push origin --delete  name
    删除本地分支：git branch -D name
    查看当前分支：git branch
    查看所有分支：git branch -a
    切换分支：git checkout name
    创建分支：git branch name

## 操作tag
    创建：git tag -a name -m “描述”
    推送：git push origin name
    删除本地：git tag -d name
    删除远程：git push origin --delete tag  name

## 改变项目引用地址
    改变：git remote set-url origin 地址
    查看：git remote -v

## 共钥、私钥
    生成共钥：ssh-keygen -t rsa -C "邮箱"
    查看公钥：cat ~/.ssh/id_rsa.pub
    查看私钥：cat ~/.ssh/id_rsa

## 杀死进程
    查看占用端口：lsof -i tcp:8080
    杀死进程：kill -9 750

## 提交文件
    查看修改的文件：git diff
    提交到暂存区：git add
    提交：git commit -m'描述'

## 本地代码回滚
    查看commit id：git log
    代码回滚：git reset --hard  commitId

## 执行一次git pull 后 获取有更改的文件列表
    git diff HEAD

## 上传本地项目到github
    1. 进入到上传的文件的目录下，使用命令初始化本地仓库
        git init
    2. 把本地仓库和远程仓库相关联，其中origin是远程仓库的别名，可以自己改变。
        git remote add origin [url]
    3. 如果远程仓库不为空，要把本地仓库和远程仓库做同步。否则可以省略此步骤，其中master为远程仓库的分支名。
        git pull --rebase origin master
    4. 提交代码
        git commit -m'描述'
    5. 把本地仓库中的文件同步到远程仓库中。其中master为远程仓库的分支名。
        git push -u origin master

## 安装包
    把名字添加到dependencies：npm install XX --save
    把名字添加到devDependencies：npm install XX -D

## node版本管理及切换----Nodejs 版本管理器
    npm install -g n
    n latest    下载最近版本的node
    n lts       下载最近最稳定的node
    n ls        列出所有的node版本
    n  版本号    切换node 版本

## nrm 管理源---npm的镜像源管理工具
    npm install -g nrm
    nrm ls            列出所有可切换的源
    nrm use taobao    切换镜像
    nrm add 仓库名     添加仓库

## 初始化package.json文件
    npm init

## git 配置项（git 默认对文件名大小写不敏感 (不区分文件名大小写)）
[原文链接](https://www.worldhello.net/gotgit/08-git-misc/030-case-insensitive.html)

    git config --list    获取git的所有配置项
    git config core.ignorecase false     配置git 使其对文件名大小写敏感