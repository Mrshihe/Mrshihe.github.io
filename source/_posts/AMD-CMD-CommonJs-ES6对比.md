---
title: AMD/CMD/CommonJs/ES6对比
date: 2018-03-08 22:04:41
tags:
  - js
---
AMD,CMD,CommonJs,ES6均是javascript模块化的规范
<!--more-->
一. CommonJs规范
CommonJs是同步加载模块,在浏览器端不适用,主要用于服务端,如NodeJs
CommonJs暴露了两个对外API,用来定义模块
```
require() // 同步引入模块
module.exports / exports // 导出需要暴露的接口
```
二. AMD规范
AMD规范用于异步加载模块,允许指定回调函数,用于浏览器端,如require.js
```
require([module], callback) // 引入模块
define(id, [depends], callback) // 定义模块
```
AMD使用时我们需要提前加载所有依赖
三. CMD规范
CMD和AMD规范类似,CMD推荐依赖就近,按需加载,用于浏览器端,如sea.js
四. ES6规范
ES6用export 导出模块, 用import导入模块
ES6模块自动采用严格模式,无需定义"use strict"
```
export default function add() {}  // 为模块指定一个默认输出, 一个模块下只能存在一个 export default
import [anyname] from '' // import导出带有default的模块时, 名字可以不对应, 不用加{}

function add() {}
export {
  add as up // export可以用as关键字重命名add方法的对外接口名
}
import { up } from '' //此时导出的up其实就是定义的add方法
import { up as up_new } from '' //import也可以用as关键字重命名方法 此时up_new 就是定义的的add()方法
```