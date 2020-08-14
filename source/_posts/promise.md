---
title: promise
date: 2020-08-14 11:22:56
tags:
---

### 一、手写一个prmose(不能链式调用)
``` bash
    function MyPromise(fn) {
        let self = this
        self.status = 'pedding'
        self.value = ''
        // 存储then的两个函数
        self.callback = []
        function resolve(value) {
            if (self.status === 'pedding') {
                self.status = 'fulfulled'
                self.value = value
                if (self.callback.length > 0) {
                    try {
                        self.callback[0].onFulFilled(value)
                    } catch (err) {
                        self.callback[0].onRejected(err)
                    }
                }
            }
        }
        function reject(value) {
            if (self.status === 'pedding') {
                self.status = 'rejected'
                self.value = value
                if (self.callback.length > 0) {
                    try {
                        self.callback[0].onRejected(value)
                    } catch (err) {
                        self.callback[0].onRejected(err)
                    }
                }
            }
        }
        try {
            fn(resolve, reject)
        } catch (err) {
            reject(err)
        }
    }
    MyPromise.prototype.then = function(onFulFilled, onRejected) {
        let self = this
        if (typeof onFulFilled !== 'function') {
            onFulFilled = () => {}
        }
        if (typeof onRejected !== 'function') {
            onRejected = () => {}
        }
        if ( self.status === 'pedding') {
            self.callback.push({
                onFulFilled,
                onRejected
            })
        }
        if ( self.status === 'fulfulled') {
            setTimeout(function() {
                try {
                    onFulFilled(self.value)
                } catch (err) {
                    onRejected(err)
                }
            })
        }
        if ( self.status === 'rejected') {
            setTimeout(function() {
                try {
                    onRejected(self.value)
                } catch (err) {
                    onRejected(err)
                }
            })
        }
    }
    // 使用
    let p2 = new MyPromise(function(resolve, reject){
        setTimeout(function() {
            resolve('成功')
        }, 1000)
    })
    p2.then(function(data){
        console.log('66', data)
    }, function(reason){
        console.log(reason)
    })
```
### 二、支持链式调用的promise
```bash
    function MyPromise(fn) {
        let self = this
        self.status = 'pedding'
        self.value = ''
        // 存储then的两个函数
        self.callback = []
        function resolve(value) {
            if (self.status === 'pedding') {
                self.status = 'fulfulled'
                self.value = value
                if (self.callback.length > 0) {
                    self.callback[0].onFulFilled(value)
                }
            }
        }
        function reject(value) {
            if (self.status === 'pedding') {
                self.status = 'rejected'
                self.value = value
                if (self.callback.length > 0) {
                    self.callback[0].onRejected(value)
                }
            }
        }
        try {
            fn(resolve, reject)
        } catch (err) {
            reject(err)
        }
    }
    MyPromise.prototype.then = function(onFulFilled, onRejected) {
        let self = this
        if (typeof onFulFilled !== 'function') {
            onFulFilled = () => self.value
        }
        if (typeof onRejected !== 'function') {
            onRejected = () => self.value
        }
        function parse(result, resolve, reject) {
            try {
                if (result instanceof MyPromise) {
                    result.then(resolve, reject)
                } else {
                    resolve(result)
                }
            } catch(err) {
                reject(err)
            }
        }
        return new MyPromise(function(resolve, reject){
            if ( self.status === 'pedding') {
                self.callback.push({
                    onFulFilled: (value) => {
                        try {
                            let result = onFulFilled(value)
                            if (result instanceof MyPromise) {
                                result.then(resolve, reject)
                            } else {
                                resolve(result)
                            }
                        } catch(err) {
                            reject(err)
                        }
                    },
                    onRejected: (value) => {
                        try {
                            let result = onRejected(value)
                            if (result instanceof MyPromise) {
                                result.then(resolve, reject)
                            } else {
                                resolve(result)
                            }
                        } catch(err) {
                            reject(err)
                        }
                    }
                })
            }
            if ( self.status === 'fulfulled') {
                setTimeout(function() {
                    try {
                        let result = onFulFilled(self.value)
                        if (result instanceof MyPromise) {
                            result.then(resolve, reject)
                        } else {
                            resolve(result)
                        }
                    } catch (err) {
                        reject(err)
                    }
                })
            }
            if ( self.status === 'rejected') {
                setTimeout(function() {
                    try {
                        let result = onRejected(self.value)
                        if (result instanceof MyPromise) {
                            result.then(resolve, reject)
                        } else {
                            resolve(result)
                        }
                    } catch (err) {
                        reject(err)
                    }
                })
            }
        })
    }
    let p2 = new MyPromise(function(resolve, reject){
        setTimeout(function() {
            reject('成功')
        }, 1000)
    })
    p2
    .then(function(data){
        return new MyPromise(function(resolve, rejected){
            reject('hahahahahahhh')
        })
    }, function(reason){
        return new MyPromise(function(resolve, rejected){
            resolve('hahaha5555555hahahhh')
        })
    })
    .then(function(data){
        console.log('p2', data)
    }, function(reason){
        console.log('p2 是', reason)
    })
```
### 三、支持链式调用的promise（去除冗余代码）
```bash
    function MyPromise(fn) {
        let self = this
        self.status = 'pedding'
        self.value = ''
        // 存储then的两个函数
        self.callback = []
        function resolve(value) {
            if (self.status === 'pedding') {
                self.status = 'fulfulled'
                self.value = value
                if (self.callback.length > 0) {
                    self.callback[0].onFulFilled(value)
                }
            }
        }
        function reject(value) {
            if (self.status === 'pedding') {
                self.status = 'rejected'
                self.value = value
                if (self.callback.length > 0) {
                    self.callback[0].onRejected(value)
                }
            }
        }
        try {
            fn(resolve, reject)
        } catch (err) {
            reject(err)
        }
    }
    MyPromise.prototype.then = function(onFulFilled, onRejected) {
        let self = this
        if (typeof onFulFilled !== 'function') {
            onFulFilled = () => self.value
        }
        if (typeof onRejected !== 'function') {
            onRejected = () => self.value
        }
        function parse(result, resolve, reject) {
            try {
                if (result instanceof MyPromise) {
                    result.then(resolve, reject)
                } else {
                    resolve(result)
                }
            } catch(err) {
                reject(err)
            }
        }
        return new MyPromise(function(resolve, reject){
            if ( self.status === 'pedding') {
                self.callback.push({
                    onFulFilled: (value) => {
                        let result = onFulFilled(value)
                        parse(result, resolve, reject)
                    },
                    onRejected: (value) => {
                        let result = onRejected(value)
                        parse(result, resolve, reject)
                    }
                })
            }
            if ( self.status === 'fulfulled') {
                setTimeout(function() {
                    let result = onFulFilled(self.value)
                    parse(result, resolve, reject)
                })
            }
            if ( self.status === 'rejected') {
                setTimeout(function() {
                    let result = onRejected(self.value)
                    parse(result, resolve, reject)
                })
            }
        })
    }
    let p2 = new MyPromise(function(resolve, reject){
        setTimeout(function() {
            reject('成功')
        }, 1000)
    })
    p2
    .then(function(data){
        return new MyPromise(function(resolve, rejected){
            reject('hahahahahahhh')
        })
    }, function(reason){
        return new MyPromise(function(resolve, rejected){
            resolve('hahaha5555555hahahhh')
        })
    })
    .then(function(data){
        console.log('p2', data)
    }, function(reason){
        console.log('p2 是', reason)
    })
```