---
title: css-grid
date: 2019-06-24 11:38:46
tags:
---

## 一、网格布局-Grid
1、最强大的 CSS 布局方案
2、它将网页划分成一个个网格, 可以任意组合不同的网格, 做出各种各样的布局。以前, 只能通过复杂的 CSS 框架达到的效果

## 二、基本概念
1、容器：采用网格布局的区域
2、项目：容器内部采用网格布局的子元素, 不包含项目的子元素

``` bash
<div class='container'>
    <div><p>1</p></div>
    <div><p>2</p></div>
    <div><p>3</p></div>
    <div><p>4</p></div>
</div>
```

``` bash
上述代码,最外层的<div>就是容器, 内层的三个<div>元素就是项目, <p>元素不属于项目
``` 
3、行: 容器里的水平区域称为行
4、列: 容器里的垂直区域称为列
5、单元格：行和列的交叉区域
正常情况下, n行和m列会产生n x m个单元格
6、网格线: 划分网格的线, 水平网格线划分出行, 垂直网格线划分出列
正常情况下, n行有n + 1根水平网格线, m列有m + 1根垂直网格线

## 三、属性之容器属性-定义在容器上的属性
## 四、属性之项目属性-定义在项目上的属性


