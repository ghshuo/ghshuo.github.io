---
title: sort用法总结
date: 2018-03-01 12:19:56
categories: "sotr" #文章分类目录
tags: javaSrcipt #文章标签 可以省略
---
![2017年年度总结](/images/sort.jpg)
<!-- more -->
### 前言
sort() 方法用于对数组的元素进行排序,并返回数组。
### 随机数
返回介于 0（包含） ~ 1（不包含） 之间的一个随机数： Number
```js
 var arr1 = Math.random();
``` 
在本例中，我们将取得介于 1 到 100 之间的一个整数：

```js
var arr2 = Math.floor((Math.random() * 100) + 1);
```
### 随机排序
```js
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.sort(function() {
    return Math.random() - 0.5; // 随机选取
})
```
### 升降排序
语法：arrayObject.sort(sortby)；
#### 升序排序
```js
function sortNumber(a, b) {
    return a - b
}
var arrSort = new Array(6);
arrSort[0] = "10";
arrSort[1] = "100";
arrSort[2] = "40";
arrSort[3] = "15";
arrSort[4] = "10000";
arrSort[5] = "1";
document.write(arrSort + "<br/>");
document.write(arrSort.sort(sortNumber));
//  1,10,15,40,100,10000
```

#### 降序排序
```js
function sortNumber(a, b) {
    return b - a 
}
var arrSort = new Array(6);
arrSort[0] = "10";
arrSort[1] = "100";
arrSort[2] = "40";
arrSort[3] = "15";
arrSort[4] = "10000";
arrSort[5] = "1";
document.write(arrSort + "<br/>");
document.write(arrSort.sort(sortNumber));

//  10000,100,40,15,10,1
```
### 数组对象某个属性排序
```js
var arr4 = [{
        name: 'ping',
        age: 2
    }, {
        name: 'guo',
        age: 10
    }, {
        name: 'hao',
        age: 5
    }];

    function arrSort(num) {
        return function(a, b) {
            var value1 = a[num];
            var value2 = b[num];
            return value1 - value2;
        }
    }
    console.log(arr4.sort(arrSort('age')));
```
![数组对象某个属性排序](/images/sort1.png)