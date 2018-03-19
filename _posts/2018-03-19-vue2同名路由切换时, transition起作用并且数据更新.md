---
layout: post
published: true
title: Vue + Vue-router 同名路由切换数据不更新的方法,transition 起作用
category: web
tags: 
  - [vue]
excerpt: vue2由于路由组件的复用问题, 相同路由切换，数据不会更新，transition不起作用，只有通过以下办法解决

---


# vue2 如何让同名路由切换时, transition 起作用?

由于路由组件的复用问题, 相同路由切换, 是不会出现动画效果的, 比如从 /article/1 切换到 /article/2

vue1 可以用路由钩子canReuse来取消组件的复用, 然后 vue2 已经取消了这个钩子, 那么 vue2 就没办法做到了吗?

其实 vue2 要做到其实也很简单

```javascript

<transition name="move" mode="out-in">
    <router-view :key="key"></router-view>
</transition>

//我们先通过计算属性, 生成一个 key, 这个 key 必须保证每一个 url 都不一样, 所以我们直接通过$route.path来生成, 有了这个唯一的 key 之后, 我们给router-view绑上即可.

computed: {
    key() {
        let key = '';
        if(this.$route.query.router) {
            key = '_' + this.$route.query.router
        } else {
            key = this.$route.path.replace(/\//g, '_')
        }
        return key;
    }
},

```

# Vue + Vue-router 同名路由切换数据不更新的方法

在默认情况下, 同名路由之间的切换, 由于组件可以服用, 放在ready里获取数据, 是不会执行的, 有两种方法可以解决

```javascript

//注意: 该问题仅存在于 vue1

//方法1: 将数据获取放到route.data下
route: {
  data({to: {params: { page }}}) {
    return Promise.all([
      this.getApi()
    ]).then(() => {
 
    })
  }
}



//方法2: 设置route.canReuse = false, 强制组件不复用~
route: {
  canReuse() {
    return false
  }
},
ready() {
  var request = $.ajax({
    type: "POST",
    dataType: 'json',
    url: "api.php"
  });
  request.then((json) => {
    // balabala
  });
}

```


# vue2数据初始化,重置数据

Object.assign(this.$data, this.$options.data()) 实现实例的数据重置

```javascript

beforeRouteUpdate(to, from, next){
    console.log('进来加载2')                          
    Object.assign(this.$data, this.$options.data())   
    next()
},
created(){     
    console.log('进来加载1')  
},

```
