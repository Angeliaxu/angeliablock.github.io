---
title: 回顾基础-手写bind
date: "2019-11-20"
template: "post"
draft: false
slug: "/post/handwritting-bind/"
category: "JavaScript"
tags:
  - "basic JavaScript"
description: "手写一个bind函数"
socialImage: "/media/42-line-bible.jpg"
---

### apply 与 call 源码

apply 与 call 源码 98%一样，不一样之处在于传参的方式

```
Function.prototype.apply=function(context,args){
    const symbolFn = Symbol('fn');
    context = context || window
    context[symbolFn] = this;
    let result = context[symbolFn](...args);
    delete context[symbolFn]
    return result
}
```

### bind 源码

```
function Person(name, age) {
    this.name =name;
    this.age = age;
    console.log(this.home)
}
Person.prototype.say = function() {
    console.log(this.name);
}

var obj = {
    home: 'home'
}
var a = Person.bind(obj);
a()
var child = new a('xuchang', 18);
console.log(child);
child.say()

1. 返回一个新函数，上下文为传进去的。
2. 当新函数被new的时候，原型指向被bind的函数也即是Person

function bind(context) {
    let _this = this;
    let _args = Array.prototype.slice.call(argument,1);
    let transfer = function() {};

    let _bindFn = function() {
        _this.apply(this instanceof _bindFn ? this : context, [..._args, ...argument])
    };
    transfer.prototype =  _this.prototype;
    _bindFn.prototype = new transfer();
    return _bindFn;
}
```
