---
title: ES6入门
date: 2019-06-01 10:36:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - share 
description: 
    ES6入门教育
---

### 写在前面的话
学习的初衷是开始看到这个名词，我的感觉竟然是蒙的。这怎么可能，虽说我不是那种前端的技术大牛，但好歹也算是会用jQuery的吧，这东西我竟然说不出一个子丑寅卯来，是不是有点说不过去。我倒不怕被笑话，你知道怎么了，我看过之后我也知道了。而且我还比你知道的多的多呢？
### ES6

一边在感慨时间不够用的同时，一边在浪费着生命！从小事儿做起，从点滴出发！
[ECMAScript 6 入门](http://es6.ruanyifeng.com/)
没看完，看了一小点，先当个松鼠吧。有兴趣的可以仔细的读一读，作者也是特别的好，怕你花不钱，就做了开源的电子书。对ES6的初步理解呢，就是一个javascript发行的最新版本，相比ES5无论是性能、功能、扩展性方面都做了大步的提升，当然不同的标准是可以进行转换的（这里的转换指的是从高版本到低版本的兼容性设置）。而这本书呢，也正在从这角度出发来系统的将这些提升都一一展示给我们，作者既然这么有心，咱们是不是也那啥一下啊？

#### 关于Promise的练习题
发下代码依次输出的内容是？
```Js
setTimeout(function () {
  console.log(1)
}, 0);
new Promise(function executor(resolve) {
  console.log(2);
  for (var i = 0; i < 10000; i++) {
    i == 9999 && resolve();
  }
  console.log(3);
}).then(function () {
  console.log(4);
});
console.log(5);
```