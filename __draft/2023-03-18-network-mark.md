---
layout: post
title: Test markdown
subtitle: Each post also has a subtitle
categories: markdown
tags: [test]
---

You can write regular [markdown](https://markdowntutorial.com/) here and Jekyll will automatically convert it to a nice webpage.  I strongly encourage you to [take 5 minutes to learn how to write in markdown](http://markdowntutorial.com/) - it'll teach you how to transform regular text into bold/italics/headings/tables/etc.

**Here is some bold text**

## Here is a secondary heading

Here's a useless table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


How about a yummy crepe?

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg)

It can also be centered!

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .center-block :}

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.


1、是否应该区分不同应用的流量处理
2、资源共享策略是什么
3、流量控制机制放在哪里来执行策略

互联网提供了一下策略：
* 普通的尽最大努力服务：IP
应用类型：交互式、文件传输、实时的

* 端到端的拥塞控制和错误恢复：TCP



，数据包的生命周期可以被描述为以下过程：

创建数据包：客户端创建HTTP请求数据包，其中包含请求方法、请求URL、HTTP版本、请求头部等信息。

发送数据包：客户端将HTTP请求数据包发送给Web服务器，通常使用TCP协议进行通信。

接收数据包：Web服务器接收到HTTP请求数据包，并对其进行解析和处理。

处理请求：Web服务器根据HTTP请求数据包中的内容，从服务器本地文件系统或数据库中检索请求的资源，并生成HTTP响应数据包。

发送数据包：Web服务器将HTTP响应数据包发送回客户端，通常使用TCP协议进行通信。

接收数据包：客户端接收到HTTP响应数据包，并对其进行解析和处理。

显示数据：客户端根据HTTP响应数据包中的内容，将响应数据显示在浏览器窗口中。

关闭连接：一旦响应数据被完全接收并显示，客户端和Web服务器将关闭连接。

* 传输延迟：设备把一个分组完全推入到网络链路所需要的总时间，取决于分组的长度和链路带宽
* 传播延迟：一个分组通过链路所需要的总时间，取决于信号传输的速度和距离。



《网络是怎样连接的》

