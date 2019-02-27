---
title: mac操作linux
date: 2019-02-27 11:14:31
tags:
---

## 连接linux服务器
    ssh 账户名@ip地址  （ssh root@199.12.345.678）
    ssh -p 端口号 账户名@服务器ip地址（ssh -p 10000 root@199.12.345.678）

## 上传文件到linux服务器——scp方式
    1、mac上传文件到Linux服务器
        scp -p 端口号 文件路径 用户名@服务器ip:目标路径
        如：scp -P10086 /Users/candy/testScp/src.zip root@xxx.xx.xxx.xxx:/root/testScp/server
    2、mac上传文件夹到Linux服务器，与上传文件相比多加了-r
        scp -r 文件路径 用户名@服务器ip:目标路径
        如：scp -r /Users/test/testFolder test@www.linuxidc.com:/test/
    3、Linux服务器下载文件到mac
        scp 用户名@服务器ip:文件路径 目标路径
        如：scp root@12.122.34.567:/root/node-v8.9.3-linux-x64.tar  /Users/candy
    4、Linux服务器下载文件夹到mac，与下载文件相比多加了-r
        scp -r 用户名@服务器ip:文件路径 目标路径
        如：scp -r root@12.122.34.567:/root/node-v8.9.3-linux-x64.tar  /Users/candy

## 上传文件到linux服务器——sftp方式
    选择Shell ----> 选择新建远程连接 ----> 选择安全文件传输 ----> 添加 用户名、ip---> 点击连接 ----> 会弹出窗口，然后执行命令
    命令格式：put 本地文件目录 远程主机路径
    如：put /Users/candy/testScp/src.zip /root/testScp/server

## 下载 node（7以上版本)
    方式一：
        1、执行命令  wget https://npm.taobao.org/mirrors/node/v8.11.3/node-v8.11.3-linux-x64.tar.xz
        2、解压 tar xvJf  node-v8.11.3-linux-x64.tar.xz
        3、配置node环境变量
        在根目录 执行 (软连接)  
        ln -s 原路径   目标路径
        例如：
            ln -s /root/node-v8.11.3-linux-x64/bin/node  /usr/local/bin/node
            ln -s /root/node-v8.11.3-linux-x64/bin/npm  /usr/local/bin/npm
    方式二：
        执行： curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash
        sudo yum -y install nodejs
        此命令会安装8版本中最高的那个版本


## 常用命令
    1、ls       列出文件或文件目录
    2、cd       切换目录
    3、mkdir    创建文件夹
    4、unzip ***.zip    解压 .zip文件到当前目录（若提示找不到命令unzip，需要执行yum install -y unzip zip）
    5、tar -xvf  ***.tar        解压 .tar文件到当前目录
    6、tar xvJf  ***.tar.xz     解压 .tar.xz文件到当前目录
    7、rm -r 文件夹名称 -f        强制删除文件夹 
    8、rm  文件名   删除文件
    9、cat 文件名   查看文件内容
    10、vi 文件名   进入文件,按下 i 键 ，出现 -- INSERT-- 就可以编辑了,按esc之后输入:wq, 保存并退出编辑模式 
    11、lsof -i:端口号   查看端口号 被哪个进程占用 （yum install lsof）
        kill -9 端口号   杀掉进程
    12、pwd             查看当前文件的目录
    13、lsb_release -a  获取linux操作系统
    14、df -h           磁盘空间
    15、echo $LANG      查看linux下当前使用语言命令
