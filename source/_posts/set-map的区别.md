---
title: set/map的区别
date: 2019-06-24 11:32:40
tags:
  - js
---

Set/WeakSet/Map/WeakMap 傻傻分不清楚
Set和Map是ES6中新定义的数据结构,记录一下
<!--more-->

### Set 对象允许你储存任何类型的唯一值，无论是原始值或者是对象引用
```
new Set([iterable]);
```
如果传递一个可迭代对象,它的所有元素将不重复地被添加到新的Set中. 如果不指定此参数或其值为null,则新的Set为空
>目前所有的内置可迭代对象如下：String、Array、TypedArray、Map 和 Set  [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)

向Set加入值的时候,不会发生类型转换,set内NaN和NaN是相等的
set的实例属性: size 返回set对象值的个数

set的方法:
- add(value) 在Set对象尾部添加一个元素,返回该Set对象
- clear() 移除Set对象内的所有元素
- delete(value) 移除Set的中与这个值相等的元素
- has(value) 返回一个布尔值,表示该值在Set中存在与否
- keys()/values() 返回一个新的迭代器对象,该对象包含Set对象中的按插入顺序排列的所有元素的值
- entries() 返回一个新的迭代器对象,该对象包含Set对象中的按插入顺序排列的所有元素的值的[value, value]数组.为了使这个方法和Map对象保持相似,每个值的键和值相等

#### WeakSet 对象允许你将弱保持对象存储在一个集合中
```
new WeakSet([iterable]);
```
如果传入一个可迭代对象作为参数,则该对象的所有迭代值都会被自动添加进生成的WeakSet对象中, null 被认为是 undefined

weakset的方法
- add(value)  在该 WeakSet 对象中添加一个新元素 value
- delete(value) 从该 WeakSet 对象中删除 value 这个元素
- has(value) 返回一个布尔值,表示给定的值 value 是否存在于这个 WeakSet 中

### set与weakset的区别
1. set可以存放任何类型的值,而weakset只能存放对象引用
2. set可以循环遍历,而weakset不可以遍历(WeakSet 对象中储存的对象值都是被弱引用的,垃圾回收机制不考虑 WeakSet 对该对象的应用,如果没有其他的变量或属性引用这个对象值,则这个对象将会被垃圾回收掉,不考虑该对象还存在于 WeakSet 中)

--------

### Map对象保存键值对,并且能够记住键的原始插入顺序,任何值(对象或者原始值) 都可以作为一个键或一个值
```
new Map()
```
Map中只存在唯一的键(NaN在Map中被认为是相等的)
#### Map和object区别
1. Map默认不包含任何键, Object有原型链,原型链上的键名可能是自己设置的键名冲突
2. Map的键可以是任何类型, Object的键只能是 Sting / Symbol 类型
3. Map中的键是有序的,按插入顺序返回, Object的键无序
4. Map的键值对个数通过size直接获取, Object需要手动计算
5. 频繁的操作下Map表现更好
6. Map可以直接迭代, Object需要获取它的键然后才能迭代

map的方法
- set(key, value) 设置Map对象中键的值,返回该Map对象
- delete(key) 如果 Map 对象中存在该元素,则移除它并返回true;否则如果该元素不存在则返回false
- get(key) 返回键对应的值,如果不存在,则返回undefined
- has(key) 返回一个布尔值,表示Map实例是否包含键对应的值
- keys() 返回一个新的Iterator对象,它按插入顺序包含了Map对象中每个元素的*键*
- values() 返回一个新的Iterator对象,它按插入顺序包含了Map对象中每个元素的*值*
- clear() 移除Map对象的所有键/值对

#### WeakMap 对象是一组键/值对的集合,其中的键是弱引用的. 其键必须是对象,而值可以是任意的
weakMap的方法: 
- set(key, value) 在WeakMap中设置一组key关联对象,返回这个 WeakMap对象
- get(key) 返回key关联对象,或者undefined(没有key关联对象时)
- has(key) 根据是否有key关联对象返回一个Boolean值
- delete(key) 移除key的关联对象

### Map和WeakMap的区别
1. Map的键可以是任何类型,WeakMap的键只能是对象
2. Map可以遍历,WeakMap不可以遍历

## 总结
Set
- 成员唯一,可以是任何类型,无序
- 只有值没有键,可遍历

WeakSet
- 成员只能是对象
- 成员都是弱引用,可被垃圾机制回收,不可遍历

Map
- 按插入顺序存放键值对,键可以是任何类型
- 可遍历

WebkMap
- 只能对象做为键(null除外),不可遍历
- 键被垃圾回收后,键无效