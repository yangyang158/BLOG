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

##  碰到的问题
1、linux 窗口 长时间 不动，就会自动中断
解决: 使用pm2 [原文](http://www.cnblogs.com/zhongweiv/p/pm2.html#pm2_start)
PM2 是一个带有负载均衡功能的 Node 应用的进程管理器。
安装：npm install pm2 -g  --->软连接  
``` bash
pm2相关命令： 
a、基本命令
    //app.js页面
    启动进程  pm2 start (node) app.js  //node 可以省略，如果是ts-node
    启动进程  pm2 start ts-node app.js 
    停止进程  pm2 stop app.js
    删除进程  pm2 delete app.js
    重载进程  pm2 reload app.js* 
    重启进程  pm2 restart app.js
    查看详情  pm2 show test
    清除日志  pm2 flush
    查看日志  pm2 log
    命名进程  pm2 start app.js --name device-cloud
    列出由PM2管理的所有进程信息      pm2 list
    打开实时监视器去查看资源占用情况  pm2 monit
b、运行多个项目——批量操作相关命令
    全部重载 pm2 reload all
    全部停止 pm2 stop all
    全部重启 pm2 restart all
    全部删除 pm2 delete all
注意：reload可以做到0秒宕机加载新的代码，restart则是重新启动，生产环境中多用reload来完成代码更新
c、集群——mode模式
    开发环境中mode多以fork的方式启动，生产环境中多用cluster方式启动
    pm2 start app.js -i 数字 --name test
　　表示在后台以cluster方式运行test，数字表示要启动的工作线程的数量
d、监听
    这个项默认是disabled，可以通过如下命令开启
    pm2 start app.js --name test --watch
    用处：主要更新代码后，不用重载或重启项目即可以立即让更新的代码起作用
    注意：适合在开发时用，可以省不少时间，生产环境下最好不要用
```


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
