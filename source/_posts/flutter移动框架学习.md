---
title: flutter移动框架学习
date: 2019-02-25 16:56:19
tags:
---

## 一、flutter环境搭建(Mac版)

### 认识
Flutter是谷歌的移动UI框架，可以快速在iOS和Android上构建高质量的原生用户界面。
##### 1、使用镜像
由于在国内访问Flutter有时可能会受到限制，Flutter官方为中国开发者搭建了临时镜像，可以将如下环境变量加入到系统环境变量中。
* 进入命令窗口
* 执行 vi .bash_profile
* 输入 以下 内容
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
按 esc，输入:wq, 按 enter
* 执行 source  .bash_profile

##### 2、安装Flutter SDK
从github拉取代码，在命令行中执行 以下 操作。
* git clone -b beta https://github.com/flutter/flutter.git
* 获取代码成功后，进入 fluter 目录，执行 vi .bash_profile，添加 
export PATH_TO_FLUTTER_GIT_DIRECTORY=/Users/candy/flutter(flutter的目录)
export PATH=${PATH}:${PATH_TO_FLUTTER_GIT_DIRECTORY}/bin
保存后 执行 source  .bash_profile
* 执行命令flutter doctor，命令的作用是检测还需要安装的依赖
没有出现commond not found：flutter 即是配置成功

**注意**：
a. 以上配置 flutter命令只在当前窗口下的flutter目录下生效，关闭窗口，需要重新执行source  .bash_profile
b. flutter doctor 非常慢，需耐心等待

##### 3、配置全局的 flutter 命令
* 配置系统环境变量，执行 vi .bash_profile
* 输入 以下 内容
export PATH=/Users/candy/flutter/bin:$PATH
* 保存后 执行 source .bash_profile

**注意**：
a. 以上配置 flutter命令只在当前窗口下生效，关闭窗口，需要重新执行source  .bash_profile


##### 4、Mac 每次都要执行source ~/.bash_profile 配置的环境变量才生效
解决方案：vi ~/.zshrc, 最后一行加上 source ~/.bash_profile 即可

##### 5、flutter doctor报错处理[参考原文](http://www.cnblogs.com/Free-Thinker/p/10212822.html)
a. 下载安装android studio 地址：https://developer.android.com/studio/index.html
* 安装成功后 配置系统环境变量
进入命令窗口，执行 vi .bash_profile，添加
export ANDROID_HOME="/Users/candy/Library/Android/sdk" //android sdk目录，替换为自己的
export PATH=${PATH}:${ANDROID_HOME}/tools
export PATH=${PATH}:${ANDROID_HOME}/platform-tools
保存后 执行 source  .bash_profile
* 执行 flutter doctor --android-licenses 
* flutter doctor 检测 是否还有错

b. 下载安装 Xcode(为了iOS开发)
下载地址：https://developer.apple.com/xcode/download/
**注意**: 
要为iOS开发Flutter应用程序，需要Xcode 至少9.0.0版本。
Xcode对Mac版本有要求，Xcode 9以上版本 需要Mac版本至少 10.13.2。

安装完之后，按报错的 提示 一步步执行命令即可


## 二、编辑器的配置
**推荐使用 VS Code**
1、需要安装两个插件:
    Flutter插件： 支持Flutter开发工作流 (运行、调试、热重载等)。
    Dart插件： 提供代码分析 (输入代码时进行验证、代码补全等)。
2、点击命令面板 ——> 输入flutter ——> 选择 Flutter：New Project ——> 输入项目名称，不能有大写
3、安装模拟器并调试
    点击右下角的 No Device，可以 安装模拟器
    窗口里 执行命令 flutter run 调试，按 r 键 刷新
    获取设备列表：执行 flutter emulator

## 三、添加配置项
1、在pubspec.yaml文件中的dependencies下添加 english_words: ^3.1.0
目的：避免引入无用的包

## 四、基础知识了解
##### A. lib/main.dart
此文件是每一个flutter项目的默认入口文件，也就是说每个flutter项目启动的时候，默认先运行这个文件的代码。
##### B  pubspec.yaml
此文件为项目配置文件
##### C. import 'package:flutter/material.dart';
每一个.dart文件的第一行都会导入flutter/material.dart包，这个包是Flutter实现Material Design设计风格的基础包，里面有文本输入框(Text)、图标(Icon)、图片(Image)、行排列布局(Align)、列排列布局(Column)、Decoration)、异步、动画等等等等控件，大家可以理解为网页中的按钮、标题、选项框呀等等控件库吧。
**注意**：Material Design是啥？是谷歌推出的一套视觉设计语言。比如有的APP可以换皮肤，而每一套皮肤就是一种设计语言，有古典风呀炫酷风呀极简风呀等等。
##### D. void main() => runApp(new MyApp())
这里的main()函数是Dart程序的入口，也就是说，Flutter程序在运行的时候，第一个执行的函数就是main()函数。
##### E. StatelessWidget和StatefulWidget
这是flutter最基础的的两种控件类，分别叫无状态类和有状态类。
两者的差别在于是否有状态，玩家创建的所有控件都继承自这两个控件。当你想展示的内容只需要改动控件本身的配置信息就可以实现时，例如文本、图片等，可以考虑使用无状态控件（StatelessWidget）。如果你想展示的内容是可以动态改变才能实现时，例如滚动列表、动画效果等，可以考虑使用有状态控件（StatefulWidget）。






