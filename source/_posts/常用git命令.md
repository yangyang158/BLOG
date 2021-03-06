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

## 暂存
    将当前分支修改的内容放到缓存区中, 并会自动建立一个缓存的list集合：git stash
    查看list集合下的所有缓存: git stash list
    将编号x的缓存释放出来, 释放之后该缓存还存在于list中: git stash apply @{x} 
    将当前分支的最后一次暂存的内容释放出来, 该缓存还存在于list中：git stash apply
    将当前分支的最后一次暂存的内容释放出来, 该缓存会从list中删除：git stash pop

## 改变项目引用地址
    改变：git remote set-url origin 地址
    查看：git remote -v
    单个删除：git remote remove 地址
    全部删除：git remote remove origin

## 共钥、私钥
    检查SSH keys是否存在：ls -al ~/.ssh
    生成共钥：ssh-keygen -t rsa -C "邮箱" / ssh-keygen
    查看公钥：cat ~/.ssh/id_rsa.pub
    查看私钥：cat ~/.ssh/id_rsa
**注意1**：生成公钥时，按照默认提示要输入两次密码，但是如果输入了，每次操作操作git库时都要输入密码，所以选择不输入密码
**注意2**：生成公钥时，报错 bash:ssh-keygen command not found，解决办法：配置系统环境变量Path, 添加ssh-keygen.exe的路径(默认在Git/usr/bin)


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

## git 配置项（git 默认对文件名大小写不敏感 (不区分文件名大小写)）
[原文链接](https://www.worldhello.net/gotgit/08-git-misc/030-case-insensitive.html)

    git config --list    获取git的所有配置项
    git config core.ignorecase false     配置git 使其对文件名大小写敏感

## 同步vscode配置项到其他电脑
    一、上传配置到github
        1. vscode 安装 Settings Sync 插件
        2. 登陆GitHub 进入Settings\Developer settings\Personal access tokens 界面，点击 Generate new token, 输入 Token description, 勾选 gist, 点击 Generate token。
        3. 复制生成的token，回到vscode，在任意界面按 Alt + Shift + U，在对话框中输入该token, 回车, 等待配置上传成功。
        4. 在用户设置中可以查看, 打开 setting.json, 可以看到 sync.gist 字段。
    二、在另一台电脑下载配置
        1. 安装Settings Sync插件
        2. 按快捷键Alt + Shift + d, 会弹出一个输入框, 根据提示，先输入GitHub Token, 再输入GitHub Gist, 回车后将会自动下载之前上传的配置
   
## 安装包
    把名字添加到dependencies：npm install XX --save
    把名字添加到devDependencies：npm install XX -D

## 安装cnpm
    npm install -g cnpm --registry=https://registry.npm.taobao.org

## node版本管理及切换----Nodejs 版本管理器(mac)
    npm install -g n
    n latest    下载最近版本的node
    n lts       下载最近最稳定的node
    n ls        列出所有的node版本
    n  版本号    切换node 版本

## nrm 管理源---npm的镜像源管理工具
    npm install -g nrm
    nrm ls                  列出所有可切换的源
    nrm use taobao          切换镜像
    nrm add 仓库名 地址     添加仓库

## 初始化package.json文件
    npm init

## npm set用来设置环境变量
    npm set init-author-name 'Your name'
    npm set init-author-email 'Your email'
    npm set init-author-url 'http://yourdomain.com'
    npm set init-license 'MIT'
    面命令等于为 npm init 设置了默认值，以后执行 npm init 的时候，package.json 的作者姓名、邮件、主页、许可证字段就会自动写入预设的值。这些信息会存放在用户主目录的 ~/.npmrc文件，使得用户不用每个项目都输入。如果某个项目有不同的设置，可以针对该项目运行 npm config。

## npm 清楚缓存
    npm cache verify
