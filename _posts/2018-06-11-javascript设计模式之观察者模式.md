---
layout: post
published: true
title: JavaScript设计模式之观察者模式
category: web
tags: 
  - [js]
excerpt: 其实在早期还是用jq开发的时代，有很多地方，我们都会出现发布订阅的影子，例如有trigger和on方法,再到现在的vue中，emit和on方法。他们都似乎不约而同的自带了发布订阅属性一般，让开发变得更加高效好用起来

---


# 发布订阅模式

> 作用

1.发布—订阅模式可以广泛应用于异步编程中，这是一种替代传递回调函数的方案。 比如，我们可以订阅ajax请求的error、success等事件。或者如果想在动画的每一帧完成之后做一些事情，那我们可以订阅一个事件，然后在动画的每一帧完成之后发布这个事件。在异步编程中使用发布—订阅模式，我们就无需过多关注对象在异步运行期间的内部状态，而只需要订阅感兴趣的事件发生点。<br/>

2.一个对象不用再显式地调用另外一个对象的某个接口。发布—订阅模式让两个对象松耦合地联系在一起，虽然不太清楚彼此的细节，但这不影响它们之间相互通信。当有新的订阅者出现时，发布者的代码不需要任何修改;同样发布者需要改变时，也不会影响到之前的订阅者。只要之前约定的事件名没有变化，就 可以自由地改变它们。

```javascript



var Event = (function() {
  
  // 缓存列表，存放订阅者的回调函数
  var clientList = {},
    listen, trigger, remove;

  // 订阅的消息添加进缓存列表 
  listen = function(key, fn) {
    if (!clientList[key]) {
      // 如果还没有订阅过此类消息，给该类消息创建一个缓存列表
      clientList[key] = [];
    }
    clientList[key].push(fn);
  };

  //发布消息
  trigger = function() {
    var key = Array.prototype.shift.call(arguments);// 取出消息类型，第一个参数
    var fns = clientList[key];// 拿到消息列表里的函数
    if (!fns || fns.length === 0) {
      // 如果没有绑定对应的消息
      return false;
    }
    for (var i = 0, fn; fn = fns[i++];) {
      // arguments 是 trigger 时带上的参数
      fn.apply(this, arguments);
    }
  };

  //取消订阅
  remove = function(key, fn) {
    var fns = clientList[key];
    // 如果 key 对应的消息没有被人订阅，则直接返回
    if (!fns) {
      return false;
    }
    // 如果没有传入具体的回调函数，表示需要取消key对应消息的所有订阅
    // 如果传入参数则取消对应的订阅
    if (!fn) {
      fns && (fns.length = 0);
    } else {
      for (var l = fns.length - 1; l >= 0; l--) {// 反向遍历订阅的回调函数列表 
        var _fn = fns[l];
        if (_fn === fn) {
          // 删除订阅者的回调函数
          fns.splice(l, 1);
        }
      }
    }
  };
  return {
    listen: listen,
    trigger: trigger,
    remove: remove
  };
})();

// 此时我们都使用全局的Event来进行发布订阅

// 订阅
Event.listen('squareMeter88', function(price) {
  console.log('价格= ' + price);
});

// 发布
Event.trigger('squareMeter88', 2000000);



//模块间通信
<!DOCTYPE html>
<html>
<head>
  <title></title>
</head>
<body>
  <button id="count">点我</button>
  <div id="show"></div>
  <script type="text/javascript">
    var a = (function() {
      var count = 0;
      var button = document.getElementById('count');
      button.onclick = function() {
        Event.trigger('add', count++);
      }
    })();
    var b = (function() {
      var div = document.getElementById('show');
      Event.listen('add', function(count) {
        div.innerHTML = count;
      });
    })();
  </script>
</body>
</html>

```

发布—订阅模式的优点非常明显，一为时间上的解耦，二为对象之间的解耦。它的应用非常广泛，既可以用在异步编程中，也可以帮助我们完成更松耦合的代码编写。发布—订阅模式还可以用来帮助实现一些别的设计模式，比如中介者模式。从架构上来看，无论是 MVC 还是 MVVM， 都少不了发布—订阅模式的参与，而且JavaScript本身也是一门基于事件驱动的语言。<br>

当然，发布—订阅模式也不是完全没有缺点（浪费内存）。创建订阅者本身要消耗一定的时间和内存，而且当你订阅一个消息后，也许此消息最后都未发生，但这个订阅者会始终存在于内存中。另外，发布—订阅模式虽然可以弱化对象之间的联系，但如果过度使用的话，对象和对象之间的必要联系也将被深埋在背后，会导致程序难以跟踪维护和理解。特别是有多个发布者和订阅者（b订阅a的消息并发布给c）嵌套到一起的时候，要跟踪一个bug不是件轻松的事情。