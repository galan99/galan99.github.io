---
layout: post
published: true
title: JavaScript学习笔记
category: javascript
tags: 
  - [面试]
excerpt: javascript基础知识，用来巩固技能，记录下来方便以后学习

---

## 常见问题

```code
1、原型
2、如何继承
3、promise与async awai区别
4、防抖和节流的区别
5、介绍flex布局
6、css实现垂直居中
7、实现深拷贝与浅拷贝
8、介绍观察者模式
9、通过什么做到并发请求
10、介绍service worker
11、介绍下Promise，内部实现，优缺点
12、介绍事件代理以及优缺点
13、HTTP和HTTPS的区别
14、vue里key主要是解决哪一类的问题，为什么不建议用索引index
15、从输入URL到页面加载全过程
16、对TCP三次握手和四次挥手的理解
17、bind、call、apply的区别
18、两个对象如何比较
19、Async里面有多个await请求，可以怎么优化
20、取数组的最大值（ES5、ES6）
21、手写数组去重函数
22、jsonp的介绍，为什么不能用post
23、介绍this
24、介绍闭包，使用场景
25、get和post有什么区别
26、实现一个三角形
27、
```

## javascript原型

```code

function Person() {

}
var person1 = new Person();
person1.name = 'Kevin';
console.log(person1.name)

Person 就是一个构造函数, person1是一个实例对象。
每个函数都有一个 prototype 属性, 这个属性指向了一个对象，这个对象正是调用该构造函数而创建的实例的原型，也就是person1的原型。

那什么是原型呢？你可以这样理解：每一个JavaScript对象(null除外)在创建的时候就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型"继承"属性。

```













