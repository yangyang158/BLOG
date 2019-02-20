---
title: jsvascript易混淆点
date: 2019-01-15 14:47:25
tags:
---

## 一、循环
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
2、for ... in(有一个历史问题：会把数据的属性循环出来)
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
## 二、Javascript数据类型
1、基本数据类型：boolean string number null undefined symbol
    注意
    a. null属于基本数据类型，但是typeof null = object(JS 存在的一个悠久 Bug)
    b. null === null, NaN !== NaN
2、对象转原始类型
对象在转换类型的时候，会调用内置的 [[ToPrimitive]] 函数，对于该函数来说，算法逻辑一般来说如下：
* 如果已经是原始类型了，那就不需要转换了
* 调用 x.valueOf()，如果转换为基础类型，就返回转换的值
* 调用 x.toString()，如果转换为基础类型，就返回转换的值
* 如果都没有返回原始类型，就会报错
* 当然你也可以重写 Symbol.toPrimitive ，该方法在转原始类型时调用优先级最高。
``` bash
    let a = {
        valueOf() {
            return 0
        },// a条件
        toString() {
            return '1'
        },// b条件
        [Symbol.toPrimitive]() {
            return 2
        }// c条件
    };
    1 + a;
    若 存在 a b c条件, c优先级最高——3
    若 存在 a b, a优先级最高——1
    若 a = {}, 结果——1[object Object]
```


## 三、instanceof 和 Symbol.hasInstance(es6提出)
1、 instanceof 判断是否是该构造函数的实例
2、 对象的Symbol.hasInstance属性, 指向一个内部方法， 可以实现自定义 instanceof 方法
``` bash
    ### example 1:
    function Person(){}
    const p1 = new Person();
    //判断p1 是否是 Person的实例
    //执行 p1 instanceof Person, 在语言内部，实际调用的是 Person[Symbol.hasInstance](p1)
    p1 instanceof Person;//true  
    Person[Symbol.hasInstance](p1);//true

    ### example 2: 重写静态方法
    class Even {
        static [Symbol.hasInstance](obj) {
            return Number(obj) % 2 === 0;
        }
    }
    const x = new Even();
    //原本判断x 是否为 Even的实例的方法，被改成了传入的数字%2===0
    1 instanceof Even 等价于 Even[Symbol.hasInstance](1);// false
    2 instanceof Even 等价于 Even[Symbol.hasInstance](2);// true
    x instanceof Even 等价于 Even[Symbol.hasInstance](x);//false

    ### example 3: 
    class Even {
        [Symbol.hasInstance](obj) {
            return Number(obj) % 2 === 0;
        }
        static [Symbol.hasInstance](obj) {
            return typeof obj === 'boolean';
        }
    }
    const x = new Even();

    2 instanceof Even 等价于 Even[Symbol.hasInstance](2);// false 调用 静态方法
    true instanceof Even 等价于 Even[Symbol.hasInstance](true);// true 调用 静态方法

    2 instanceof x 等价于 x[Symbol.hasInstance](2);// true  调用 动态方法

    x instanceof Even 等价于 Even[Symbol.hasInstance](x);//false  调用静态方法
```

## 四、reduce方法的使用
reduce作用：将数组中的元素通过回调函数最终转换为一个值

* 用法
1.接受2个参数，一个回调, 一个初始值
2.回调函数接受4个参数，分别是累计值、当前元素、当前索引、原数组
``` bash
let arr = ['a', 'b', 'c', 'd'];
let value = arr.reduce(function(acc, item, index, arr){
    //第一次遍历执行回调函数时，acc为初始值0, item为a, 返回a1, 
    //该结果在第二次执行回调函数时会当作第一个参数传入，依次论推
    console.log('acc', acc);//0、a1、b1、c1、d1   
    console.log('item', item);//a、b、c、d
    console.log('index', index);//0、1、2、3
    console.log('arr', arr);// ["a", "b", "c", "d"]
    return item + '1';
}, 0)
//value 是 最后一次执行回调函数return的值
console.log(value);//d1
```

* 一般方法实现：数组中的元素 求和
``` bash
let arr = [1, 2, 3];
let sum = 0;
for(let i=0;i<arr.length;i++){
    sum += arr[i];
}
console.log(sum);//6
```

* reduce实现 数组中的元素 求和
``` bash
let arr = [1, 2, 3];
let sum = arr.reduce(function(sum, item){
    return sum + item
}, 0)
console.log(sum);//6
```

* reduce实现 map函数的功能，返回新数组
``` bash
let arr = [1, 2, 3];
let newArr = arr.reduce(function(acc, item){
    acc.push(item*2);
    return acc;
}, [])
console.log(newArr);//[2, 4, 6]
```

## 获取字符串中出现最多的字符
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
