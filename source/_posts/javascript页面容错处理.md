---
title: javascript页面容错处理
date: 2020-06-03 14:56:56
tags:
---

### 一、js脚本运行出错，导致页面空白，可以监听error事件定位错误
    window.onerror = handleErr
    var txt = ""
    function handleErr(msg,url,l) {
        txt = "There was an error on this page.\n\n"
        txt += "Error: " + msg + "\n"
        txt += "URL: " + url + "\n"
        txt += "Line: " + l + "\n\n"
        txt += "Click OK to continue.\n\n"
        alert(txt)
        return false; // true：控制台不显示错误日志; false：控制台显示错误日志
    }
    可以通过img标签发送get请求 进行错误上报

### 二、捕获错误
1、try catch——可以绕开错误,代码还可以继续运行
    try {
        ...
    } catch (error) {
        // 代码出错后执行这里
        console.log('错误', error)
    } finally {
        // 不管代码是否报错，都将执行这里
        ...
    }
2、抛异常
    a、throw exception 
    b、new Error()

### 三、跨域页面捕获异常-Script error
    当引入的第三方脚本发生错误时, 该脚本中的 window.onerror 会将这类错误统一展示为 Script error 。所以为了解决这个问题一般有两种解决方案：
    a、后端配置 Access-Control-Allow-origin、前端在 script 标签配置 crossorigin
    b、劫持原生方法，使用 try/catch 绕过，将错误抛出
