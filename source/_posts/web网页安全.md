---
title: web网页安全
date: 2020-05-22 14:02:51
tags:
---

## 一、XSS 跨站脚本攻击
原理：通过在web网页里植入可执行的脚本 盗取信息
防御： 
    1、HttpOnly Cookie
    2、不信任用户输入, 对特殊字符进行转义
    3、建立白名单, 配置规则, 告诉浏览器可以加载和执行哪些资源, 设置meta标签的方式 或 HTTP Header 中的 Content-Security-Policy

## 二、CSRF(Cross Site Request Forgery)，即跨站请求伪造
原理: 盗用了用户的身份, 以用户的名义发请求
场景: 用户登录了某网站且未退出, 被诱惑访问了其他网站
防御: 
    1、检查 header中的refer字段
    2、get请求比较好伪造, get请求不要对数据做修改
    3、对账户交易的要加上验证码或token

疑问：cookie是不能跨域访问的，为什么会有csrf攻击？
浏览器会依据加载的域名附带上对应域名cookie。
就是如果用户在a网站登录且生成了授权的cookies，然后访问b网站，b站故意构造请求a站的请求，用script，img或者iframe之类的加载a站着个地址，浏览器会附带上a站此登录用户的授权cookie信息，这样就构成crsf

## 三、点击劫持
原理：一种视觉上的欺骗手段, 如：在页面上覆盖一个透明的iframe, 此时用户将在不知情的情况下点击透明的iframe里的页面
防御：
    1、设置请求头的X-FRAME-OPTIONS字段, 控制访问iframe的来源

## 四、URL跳转漏洞
原理: 攻击者做一个恶意链接，比如：qq号被盗 就是打开了一个恶意链接
eg: http://www.XX.XX.com/index?url=http://XXXXX
前面看着是安全的链接, 参数url是一个恶意网址
防御：
    1、加入token对生成的链接进行校验
    2、对referer的限制

## 五、SQL注入


