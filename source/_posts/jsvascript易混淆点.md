---
title: jsvascript易混淆点
date: 2019-01-15 14:47:25
tags:
---

## 1.循环
1、for ... of(es6提出来的方法, 可以解决for...in的遗留问题)
* 遍历字符串、数组, 直接获取值，不拿索引
* 不能遍历对象

``` bash
var str = 'asdfg';
for(let i of str){
    console.log('i', i); //'a' 's' 'd' 'f' 'g'
}
var obj = {name: '小红', age: '12'};
for(let i of obj){
    console.log('i', i); //VM60:2 Uncaught TypeError: obj is not iterable at <anonymous>:2:14
}
var arr = ['a', 'b', 'c'];
for(let i of arr){
    console.log('i', i); //'a' 'b' 'c'
}
arr.name = '小明';
console.log(arr, arr.length);//["a", "b", "c", name: "小明"]   3
for(let i of arr){
    console.log('i', i); //'a' 'b' 'c'
}
```
2、for ... in(有一个历史问题：会数据的属性循环出来)
* 遍历字符串、数组、对象
``` bash
var str = 'asdfg';
for(let i in str){
    console.log('i', i, 'str[i]', str[i]); //i 0 str[i] a    i 1 str[i] s  i 2 str[i] d ...   
}
var obj = {name: '小红', age: '12'};
for(let i in obj){
    console.log('i', i, 'obj[i]', obj[i]); //i name obj[i] 小红     i age obj[i] 12
}
var arr = ['a', 'b', 'c'];
for(let i in arr){
    console.log('i', i, 'arr[i]', arr[i]); //i 0 arr[i] a    i 1 arr[i] b   i 2 arr[i] c
}
arr.name = '小明';
console.log(arr, arr.length);//["a", "b", "c", name: "小明"]   3
for(let i in arr){
    console.log('i', i, 'arr[i]', arr[i]); //i 0 arr[i] a    i 1 arr[i] b   i 2 arr[i] c   i name arr[i] 小明
}
```

## 2.获取字符串中出现最多的字符
``` bash
var str='13rtghgfds';
var obj = {};
for(let i=0;i<=str.length-1;i++){
    console.log(str[i]);
    let key = str[i];
    if(obj[key]){
        obj[key]++;
    }else{
        obj[key] = 1;
    }
}
console.log('obj', obj) // {1: 1, 3: 1, d: 1, f: 1, g: 2, h: 1, r: 1, s: 1, t: 1}
```