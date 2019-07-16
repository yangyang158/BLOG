---
title: console用法
date: 2019-07-16 10:04:11
tags:
---

## 一、输出一个变量或者输出一个字符串

``` bash
let name1 = 'yy'
let name2 = 'zs'
console.log(name1, name2); // yy zs
```

## 二、支持占位符, 不过其支持的占位符种类较少, 只支持字符串（%s）\整数（%d或%i）、浮点数（%f）和对象（%o）

``` bash
console.log("%d年%d月%d日", 2019, 09, 22); // 2019年09月22日
```
## 三、console.log()、console.warn()、console.error() 打印的颜色不一样

``` bash
console.warn('warn'); // 黄色
console.error('error'); // 红色
console.log('log'); // 正常黑色
```

## 四、console.table() 以表格形式输出

``` bash
const people = {
    "person1": {"fname": "san", "lname": "zhang"}, 
    "person2": {"fname": "si", "lname": "li"}, 
    "person3": {"fname": "wu", "lname": "wang"}
};
console.table(people)
```
![效果](https://pic1.zhimg.com/80/v2-47e42c76d29825ad76e0f8db651be5b0_hd.png "")

## 五、console.time() 与 console.timeEnd() 测试代码运行时间

``` bash
console.time("for-test");
const arr = [];
for(let i = 0; i < 100000; i++) {
    arr.push({"key": i});
}
console.timeEnd("for-test");
// for-test: 10.705810546875ms
```

## 六、console.assert() 断言, 当表达式为 false 时，输出错误信息Assertion failed: console.assert

``` bash
let arr = [1, 2, 3, 4]
console.assert(arr.length === 2); // Assertion failed: console.assert
console.assert(arr.length === 4); // undefined
```

## 七、console.count() 计算代码被执行了多少次

``` bash
function func() {
    console.count("次数");
}
for(let i = 0; i < 3; i++) {
    func();
}
// 输出
次数: 1
次数: 2
次数: 3
```

## 八、console.group()、 console.groupEnd() 与 console.groupCollapsed() 可以展示层级关系

``` bash
console.group("1"); // 层级默认展开
console.log("1-1");
console.log("1-2");
console.log("1-3");
console.groupEnd();
```

``` bash
console.groupCollapsed("2"); // 层级默认折叠
console.log("2-1");
console.log("2-2");
console.log("2-3");
console.groupEnd();
```
![效果](https://pic3.zhimg.com/80/v2-696609e37fad3a14b054285c57172aae_hd.png "")

