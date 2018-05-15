---
title: ajax中post和get的区别
date: 2018-03-26 19:20:56
tags: [前端, JavaScript]
categories: 
        - 前端
        - JavaScript
---


### 前言
[欢迎大家去我的网站查看](http://genghongshuo.com.cn/)
### ajax中post和get的区别
get和post都是向服务器发送一种请求，只是发送机制不同
1 GET请求会将参数跟在URL后进行传递，而POST请求则是作为HTTP消息的实体内容发送给WEB服务器。
2.首先是"GET方式提交的数据最多只能是1024字节"，Post传输的数据量大，可以达到2M。
3.get方式请求数据会被浏览器缓存起来，因此其他人就可以从浏览器的历史记录中读取到这些数据，例如账号密码等。在某种情况下，get方式会带来严重的安全问题。而post方式相对来说就可以避免这些问题。
4.Post请求必须设置Content-Type值为application/x-form-www-urlencoded； 
5.发送请求时,因为Get请求的参数都在url里,所以send函数发送的参数为null,而Post请求在使用send方法时,却需赋予其参数；
### 在服务器端的区别:
在客户端使用get请求时,服务器端使用Request.QueryString来获取参数,而客户端使用post请求时,服务器端使用Request.Form来获取参数.
HTTP标准包含这两种方法是为了达到不同的目的。POST用于创建资源，资源的内容会被编入HTTP请示的内容中。例如，处理订货表单、在数据库中加入新数据行等。

当请求无副作用时（如进行搜索），便可使用GET方法；当请求有副作用时（如添加数据行），则用POST方法。一个比较实际的问题是：GET方法可能会产生很长的URL，或许会超过某些浏览器与服务器对URL长度的限制。

若符合下列任一情况，则用POST方法：

* 请求的结果有持续性的副作用，例如，数据库内添加新的数据行。
* 若使用GET方法，则表单上收集的数据可能让URL过长。
* 要传送的数据不是采用7位的ASCII编码。

若符合下列任一情况，则用GET方法：

* 请求是为了查找资源，HTML表单数据仅用来帮助搜索。
* 请求结果无持续性的副作用。
* 收集的数据及HTML表单内的输入字段名称的总长不超过1024个字符。
### 下面用代码来说明两者的区别：
#### post请求
```js
function postMethod(){

    var xhr = new createXHR();

    var userName = document.getElementById("userName").value;
    var age = document.getElementById("age").value;
    var data = "userName=" + encodeURTComponent( userName ) + "&age=" + encodeURTComponent( age );

    //不用担心缓存问题
    xhr.open( "post", "example.php", true );

    //必须设置,否则服务器端收不到参数
    xhr.setRequestHeader( "Content-Type", "application/x-www-form-urlencoded" );

    xhr.onreadystatechange = function(){

        if( xhr.readyState = 4 && xhr.status == 200 ){
            document.getElementById("result").innerHTML = xhr.responseText;
        }
    }

    //发送请求,要data数据
    xhr.send( data );

}
```
#### git请求
function getMethod(){

    var xhr = new createXHR();

    var userName = document.getElementById("userName").value;
    var age = document.getElementById("age").value;

    //添加参数,以求每次访问不同的url,以避免缓存问题
    xhr.open( "get", "example.php?userName=" + encodeURTComponent( userName ) + "&age=" + encodeURTComponent( age ) + "&random=" + Math.random(), true );

    xhr.onreadystatechange = function(){

        if( xhr.readyState == 4 && xhr.status == 200 ){

            document.getElementById("result").innerHTML = xhr.responseText;
        }
    }

    //发送请求,参数为null
    xhr.send( null );
}

