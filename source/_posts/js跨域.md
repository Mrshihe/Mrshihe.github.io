---
title: js跨域
date: 2019-06-08 19:05:27
tags:
  - js
---
说起跨域,就要先从浏览器的同源策略说起,所谓同源策略就是指协议,域名,端口号都要相同,一项不同就产生了跨域问题。
同源策略作用大体分为两类: (1)限制接口 (2)限制查询DOM
<!--more-->
解决跨域问题,常见的有已下几种方法:
一、jsonp
原理: 同源策略中,html中通过相应标签从不同域名中加载静态资源文件是被浏览器允许的,所以script/img/iframe标签是例外,不受同源策略限制
```
let script = document.createElement('script');
script.src = '***?id=3&callback=callback';
document.body.appendChild(script);

function callback(res) { console.log(res) }
```
缺点: 只支持GET请求
二、CORS(Cross-origin resource sharing) 跨域资源共享
CORS分为简单请求和非简单请求:
同时满足以下两个条件,就属于简单请求:
1.请求方法是下列之一: HEAD,GET,POST
2.HTTP头部信息不超过以下几种字段:
(1) Accept (2) Accept-Language (3) Content-Language (4) Last-Event-ID
(5) Content-Type 只限于application/x-www-form-urlencoded、multipart/form-data、text/plain
- 简单请求会自动在请求头中加入Origin字段
- 非简单请求会先发起一次预检请求

[cors参考资料](http://www.ruanyifeng.com/blog/2016/04/cors.html)
三、Nginx代理
```
server {
  listen 0000;
  server_name localhost
  location ^~ /api {
    proxy_pass http://******
  }
}
// 凡是localhost:0000/api的所有接口都转发到http://******上处理
```

四、document.domain + iframe
原理:通过设置document.domain为统一主域名
```
// 现有abc.test.com下的a.html和xyz.test.com下的x.html
// a.html
<iframe src="xyz.test.com/x.html"></iframe>
<script>
document.domain = 'test.com'
iframe.onload = function(){ //这里就可以查询x.html页面DOM }
</script>
//x.html
<script>
document.domain = 'test.com'
</script>
```
缺点:只支持主域名相同的
五、postMessage,H5新增API,可以安全地实现跨源通信
```
otherWindow.postMessage(message, targetOrigin, [transfer])
```
otherWindow 其他窗口的引用,比如iframe的contentWindow属性、执行window.open返回的窗口对象、或者是命名过或数值索引的window.frames
message 将要发送到其他 window的数据
targetOrigin 通过窗口的origin属性来指定哪些窗口能接收到消息事件,其值可以是字符串"*"（表示无限制）或者一个URI
transfer(可选) 是一串和message同时传递的Transferable对象.这些对象的所有权将被转移给消息的接收方,而发送一方将不再保有所有权
```
// http://a.test.com
<iframe name="toiframe" src="http://b.example.com"></iframe>
// 用于发送信息
window.frames['toiframe'].postMessage('from a.test.com','http://b.example.com')
// 用于接收信息
window.addEventListener('message', (e) => {
  // 检验消息来源
  if(e.origin === 'http://a.test.com'){

  }
})

// http://b.example.com
window.addEventListener('message', (e) => {
  if(e.origin === 'http://a.test.com'){
    // 接收消息并回应
    e.source.postMessage('from b.example.com answer', e.origin)
  }
})
```
