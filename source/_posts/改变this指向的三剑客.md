---
title: 改变this指向的三剑客
date: 2018-06-04 16:18:02
tags:
    - js
---
记录一下能改变this指向的三剑客,他们就是call() / apply() / bind()
call()/apply()作用一致,只是call接受的是一个参数列表,apply接受一个包含多个参数的数组,bind则创建一个新的函数
<!--more-->
一. call()方法 使用一个指定的 this 值和单独给出的一个或多个参数来调用一个*函数*
```
funtion.call(thisArg, arg1, arg2, ...)
```
thisArg可选参数, 在function运行时使用的this值. 非严格模式下,指定null或undefined则自动替换成全局对象, 如果不指定thisArg,this的值会绑定为全局对象,严格模式下this的值是undefined

举个例子
```
var apple = {
  color: 'red',
  say: function() {
    return `color is ${this.color}`
  }
}
var banana = {
  color: 'yellow'
}
apple.say.call(banana) // color is yellow
```
banana没有say方法,我们可以去向apple借用一下,再比如,取出数组中的最大值,我们可以用一个简单的方法,借用Math.max方法
```
var numbers = [5,8,90]
Math.max.call(numbers,...numbers) // 90
```
其实说白了, call他自己做了三件事
1. 将函数设置为传入对象的属性
2. 然后执行这个函数
3. 执行完成后,删除这个新增属性

那我们就可以实现一个自己的call函数
```
Function.prototype.mycall = function(context=window){
  context.fn = this //将函数设置为传入对象的属性
  let args = [...arguments].slice(1) //抽离出传入的参数
  let result = context.fn(...args) // 执行这个函数
  delete context.fn // 删除这个属性
  return result
}
```
ES5版相对复杂
```
Function.prototype.mycall = function(context){
  context = context || window
  var args = []
  for(var i=0; i<arguments.length; i++){
    args.push('arguments[' + i + ']')
  }
  var argstr = args.join(',')
  context.fn = this
  // ES5 由于无法通过解构的形式传入参数，只能通过字符串拼接然后再通过eval来执行
  var res =  eval('context.fn(' + argstr + ')')
  delete context.fn
  return res
}
```
eval()函数可计算某个字符串,并执行其中的的JavaScript代码**不建议使用**
*需要注意的点*
- 如果string参数不是原始字符串,那么该方法将不作任何改变地返回
- 试图覆盖eval属性或把eval()方法赋予另一个属性,并通过该属性调用它,则抛出一个EvalError异常
- 参数中没有合法的表达式和语句,则抛出SyntaxError异常
- 传递给eval()的Javascript代码生成了一个异常,eval()将把该异常传递给调用者

二、apply()方法与call()功能一致
写一个自己的apply方法
```
Function.prototype.myapply = function(context = window) {
  context.fn = this
  let result;
  if(arguments[1]) { // 判断是否有第二个参数
    result = context.fn(...arguments[1])
  } else {
    result = context.fn()
  }
  delete context.fn
  return result
}
```
ES5版
```
Function.prototype.myapply = function(){
  context = context || window; 
  context.fn = this;
  var result;
  // 判断是否存在第二个参数
  if (arguments[1]) {
    result = context.fn();
  } else {
    var args = [];
    for (var i = 0, len = arguments[1].length; i < len; i++) {
      args.push('arguments[1][' + i + ']');
    }
    result = eval('context.fn(' + args + ')');
  }
  delete context.fn
  return result;
}
```
三、bind()方法创建一个新的函数,在bind()被调用时,这个新函数的this被指定为bind()的第一个参数,而其余参数将作为新函数的参数,供调用时使用
```
function.bind(thisArg, arg1, arg2)
```
thisArg 调用绑定函数时作为this参数传递给目标函数的值;如果使用new运算符构造绑定函数,则忽略该值
如果bind函数的参数列表为空,或者thisArg是null或undefined,执行作用域的this将被视为新函数的thisArg
```
var number = 9
var obj = {
  number: 8,
  getNumber: function(){
    return `number:${this.number}`
  }
}
obj.getNumber() // number:8
var newGet = obj.getNumber
newGet() //number:9  因为函数是在全局作用域中调用的
var bindGet = newGet.bind(obj)
bindGet() //number:8 通过bind改变this的值
var emptyBind = newGet.bind() 
emptyBind() //number:9 因为bind参数列表为空,emptyBind在全局作用域中调用,所以this指向window thisArg为null和undefined时同理
```
bind另一个用法是可以使一个函数拥有预设的初始参数
```
function list() {
  return Array.prototype.slice.call(arguments)
}
var newlist = list.bind(null,37)
newlist(1,2,3) // [37,1,2,3]
```
手写一个bind方法
```
Function.prototype.mybind = function(context){
  let that = this;
  // mybind绑定的时候传入的第二个参数
  let args1 = Array.prototype.slice.call(arguments,1);
  let bindFn = function(){
    let args2 = Array.prototype.slice.call(arguments);
    // 判断一下是否用new创建了一个新实例
    return that.apply(this instanceof bindFn?this:context,args1.concat(args2)); 
  }
  // 使用空函数继承原型链
  let Fn = function(){};
  Fn.prototype = this.prototype;
  bindFn.prototype = new Fn();
  return bindFn;
}
```