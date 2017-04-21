---
layout: post
published: true
title: Vue2 + Vue-Router2 如何实现懒加载
category: web
tags: 
  - [vue]
excerpt: 当打包构建应用时，Javascript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了

---


# 路由懒加载

当打包构建应用时，Javascript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。


> 分组写法1

```javascript

const centerHome = r => require.ensure([], () => r(require('../pages/home/center-home.vue')), 'center')
const centerInfo = r => require.ensure([], () => r(require('../pages/home/center-info.vue')), 'center')
const centerShop = r => require.ensure([], () => r(require('../pages/home/center-shop.vue')), 'center')

const router = new VueRouter({
    mode: 'hash',
    base: __dirname,
    scrollBehavior,
    routes: [
        { name:'center-home', path: '/center/home', component: centerHome },
        { name:'center-info', path: '/center/info', component: centerInfo },
        { name:'center-shop', path: '/center/shop', component: centerShop },
    ]
})

```
>分组写法2

```javascript

let router = new Router({
	mode :"history",
	routes: [
		{
            path: '/',
            redirect: '/login'
        },
        {	
        	name:'login',
            path: '/login',
            component: resolve => require(['../views/login.vue'], resolve)
        },
		{	
			name:'index',
			path: '/index',
            component: resolve => require(['../components/Hello.vue'], resolve)
		},
		{
			name:'list',
			path: '/list',
            component: resolve => require(['../views/list.vue'], resolve)
		},
		{	
			name:'other',
			path: '/other/:id',
            component: resolve => require(['../views/other.vue'], resolve)
		},
		{
			path: "*",
			redirect: "/"
		}
	],
	scrollBehavior(to, from, savedPosition) {
		if (savedPosition) {
			return savedPosition
		}
		if (to.hash) {
			return {
				selector: to.hash
			}
		}
		return {
			y: 0
		}
	}
})

```

> webpack1的写法

```javascript

// vue1
'/group/log': {
    name: 'grouplog',
    component(resolve) {
        require(['./components/group/group-log.vue'], resolve)
    }
}
// vue2
const Foo = resolve => require(['./Foo.vue'], resolve)
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})


```



> 升级webpack2后遇到的问题, webpack2修改了api, (准确来说是: webpack2 已经支持原生的 ES6 的模块加载器了)所以您需要这么写:

```javascript

// vue1
const Question = () => System.import('./components/question/index.vue')
// 中间代码省略
'/group/log': {
    name: 'grouplog',
    component: Question
}

// vue2
const Question = () => System.import('./components/question/index.vue')
const router = new VueRouter({
    mode: 'history',
    base: __dirname,
    routes: [
        { path: '/question/:id', component: Question },
    ]
})

```
