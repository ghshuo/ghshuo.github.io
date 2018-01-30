---
title: mixins混入选项操作
date: 2018-01-30 20:10:50
categories: "vue" #文章分类目录
tags: vue #文章标签 可以省略
description: #你對本頁的描述 可以省略
---
### 概念
mixins 选项接受一个混合对象的数组。这些混合实例对象可以像正常的实例对象一样包含选项，他们将在 Vue.extend() 里最终选择使用相同的选项合并逻辑合并。

### 用途
Mixins一般有两种用途：

1、在你已经写好了构造器后，需要增加方法或者临时的活动时使用的方法，这时用混入会减少源代码的污染。
2、很多地方都会用到的公用方法，用混入的方法可以减少代码量，实现代码重用。

### 基本用法
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>mixins混入选项操作</title>
  <script src="js/vue.js"></script>
</head>

<body>
    <div id="app">
        <p>num:{{ num }}</p>
        <P><button @click="add">增加</button></P>
    </div>
    <script type="text/javascript">
        // 临时添加需求 需要混入
        var addlogin={
            updated:function(){
                console.log("数据放生变化,变化成"+this.num+".");
            }
        };
        // 构造器
        var app=new Vue({
            el:'#app',
            data:{
                num:1
            },
            methods:{
                add:function(){
                    this.num++;
                }
            },
            updated:function(){
              console.log('我是原生的undated');
            },
            mixins:[addlogin] // 混入   可以混入多个
            // 混入先执行、原生的后执行
        })
    </script>
</body>

</html>
```
### 全局API混入方式
全局混入的执行顺序要前于混入和构造器里的方法
```js
Vue.mixin({
    updated:function(){
        console.log('我是全局被混入的');
    }
})
```
### 组件
混合 (mixins) 是一种分发 Vue 组件中可复用功能的非常灵活的方式。混合对象可以包含任意组件选项。当组件使用混合对象时，所有混合对象的选项将被混入该组件本身的选项。
示例
```js
// 定义一个混合对象
  var myMixin = {
    created: function () {
      this.hello()
    },
    methods: {
      hello: function () {
        console.log('hello from mixin!')
      }
    }
  }

  // 定义一个使用混合对象的组件
  var Component = Vue.extend({
    mixins: [myMixin]
  })

  var component = new Component() // => "hello from mixin!"
```