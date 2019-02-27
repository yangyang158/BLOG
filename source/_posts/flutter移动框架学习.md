---
title: flutter移动框架学习
date: 2019-02-25 16:56:19
tags:
---

## 一、flutter环境搭建(Mac版)
1、使用镜像
由于在国内访问Flutter有时可能会受到限制，Flutter官方为中国开发者搭建了临时镜像，可以将如下环境变量加入到系统环境变量中。
* 进入命令行
* 执行 vi .bash_profile
* 输入 以下 内容
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
按 esc，输入:wq, 按 enter
* 执行 source  .bash_profile

2、安装Flutter SDK
从github拉取代码，在命令行中执行 以下 操作。
* git clone -b beta https://github.com/flutter/flutter.git
* 获取代码成功后，进入 fluter 目录，执行 vi .bash_profile，添加 
export PATH_TO_FLUTTER_GIT_DIRECTORY=/Users/candy/flutter(flutter的目录)
export PATH=${PATH}:${PATH_TO_FLUTTER_GIT_DIRECTORY}/bin
保存后 执行 source  .bash_profile
* 执行命令flutter doctor，命令的作用是检测还需要安装的依赖
没有出现commond not found：flutter 即是配置成功

**注意**：
a. 以上配置 flutter命令只在flutter目录下生效
b. flutter doctor 非常慢，需耐心等待

3、配置全局的 flutter 命令
* 配置系统环境变量，执行 vi .bash_profile
* 输入 以下 内容
export PATH=/Users/candy/flutter/bin:$PATH
* 保存后 执行 source  .bash_profile


3、flutter doctor报错处理[原文](http://www.cnblogs.com/Free-Thinker/p/10212822.html)
a. 下载安装android studio 地址：https://developer.android.com/studio/index.html
* 安装成功后 配置系统环境变量
执行 vi .bash_profile，添加
export ANDROID_HOME="/Users/candy/Library/Android/sdk" ////android sdk目录，替换为自己的
export PATH=${PATH}:${ANDROID_HOME}/tools
export PATH=${PATH}:${ANDROID_HOME}/platform-tools
保存后 执行 source  .bash_profile

b. 下载安装 Xcode(为了iOS开发)




