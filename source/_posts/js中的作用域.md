---
title: js中的作用域
date: 2019-01-15 10:47:15
tags:
---

## var、let、const
* var、函数 存在变量提升，并且 函数提升 优于 变量提升
``` bash
console.log(a);//function a(){}
function a(){};
var a = false;
```
* 全局作用域下使用 let 和 const 声明变量，变量并不会被挂载到 window 上
``` bash
console.log(window.a); // undefined
var a = 1;
let b = 1;
const c = 1;
console.log(window.a); // 1
console.log(window.b); // undefined
console.log(window.c); // undefined
```

* const 声明过的变量不能重新赋值,let声明过的变量可以重新赋值, var可以重复声明变量，新声明的会覆盖之前的变量值
``` bash
let a = 1;
let a = 2;//VM1286:1 Uncaught SyntaxError: Identifier 'v' has already been declared

var b = 1;
var b = 2;//b = 2
```

为什么会存在变量提升呢, 原因是为了解决函数间互相调用的情况
``` bash
function test1() {
    test2()
}
function test2() {
    console.log(11)
}
test1();
假如不存在提升这个情况，那么就实现不了上述的代码
```

## 理解执行上下文和作用域
* 作用域：基于函数，当定义函数的时候确定
* 执行上下文：基于对象，主要是关键字this的指向，由调用函数的时候决定

## apply()、call()、bind()
* apply、call：相同点：在调用时 设置函数体内this对象的值；不同点：接收参数的方式不同
* bind：定义时绑定this对象，调用时在传参

1. 例子——apply、call
``` bash
let obj1 = {
    words: '你好!',
    speak: function(name1, name2){
        console.log(name1 + ' ' + name2 + ', ' + this.words);
    }
}
let obj2 = {
    words: '你吃饭了吗？'
}
obj1.speak('小红', '小丽');//this的指向为obj1。输出结果：小红 小丽, 你好!
obj1.speak.call(obj2, '小明', '小刚');//将this的指向改成了obj2。输出结果：小明 小刚, 你吃饭了吗？
obj1.speak.apply(obj2, ['小明', '小刚']);//将this的指向改成了obj2。输出结果：小明 小刚, 你吃饭了吗？
```

2. 例子——bind
``` bash
let obj3 = {
    words: '你吃饭了吗？'
}
let obj4 = {
    words: '你好!',
    speak: function(name){
        console.log(name + ', ' + this.words);
    }.bind(obj3)//将this的指向改成了obj3。输出结果：小刚, 你吃饭了吗？
}

obj4.speak('小刚');
```

``` bash
var num = 10;
var obj5 = {
    num: 12,
    add: function(){
        setTimeout(function(){
            console.log('num', this.num)
        }, 1000)
    }
}
obj5.add();//setTimeout中this默认为window。输出结果：10


var obj5 = {
    num: 12,
    add: function(){
        setTimeout(function(){
            console.log('num', this.num)
        }.bind(this), 1000);
    }
}
obj5.add();//bind将this指向obj5。输出结果：12
```
3. 例子——多次调用bind
* 不管给函数 bind 了多少次，永远是由第一次 bind 决定的
``` bash
let obj6 = {};
let func = function(){
    console.log(this);
}
func.bind(obj6)();//this指向obj6
func.bind(obj6).bind()();//this指向obj6
func.bind().bind(obj6)();//this指向window
```

## 普通函数和箭头函数比较
* es6的箭头函数的出现，导致很少用到call、bind、apply函数
* call、bind、apply函数不能改变箭头函数的this
* 箭头函数的this：如果包裹在函数中,只取决包裹箭头函数的第一个普通函数的this；如果放在全局中就是指全局对象window。换句话说内部的this就是外层代码块的this
* 普通函数：函数调用时所在的对象

``` bash
const foo = function() {
    return () => {
        console.log(this === window)//由foo函数的作用域决定，this为{a:1}。输出结果：false
    }
}
foo.call({a:1})()

const foo2 = ()=>{
    return () => {
        console.log(this === window)//foo2也是箭头函数，this为window。输出结果：true
    }
}
foo.call({a:1})()
```

* 箭头函数 如果包裹在函数中, 只取决包裹箭头函数的第一个普通函数的this
``` bash
function foo4() {
    return function(){//箭头函数由该匿名函数的this决定
        return () => {
            return () => {
                console.log('this', this, 'id:', this.id);
                
            };
        };
    };
}
var f = foo4.call({id: 1});
f()()();//this指向window  id为undefined
f.call({id:5})()();//this指向{id:5}  id为5
f()().call({id:10})();//this指向window  id为undefined
```

``` bash
function foo5() {//箭头函数由foo5函数的this决定
    return () => {
        return () => {
            return () => {
                console.log('this', this, 'id:', this.id);
                
            };
        };
    };
}
var f = foo5.call({id: 1});
f()()();//this指向{id: 1}  id为1
f.call({id:5})()();//this指向{id:1}  id为1
```



