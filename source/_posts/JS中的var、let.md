---
title: JS中的var、let
date: 2022-10-30 14:29:42
tags: JS高级
index_img: /img/1.jpg
banner_img: /img/1.jpg
comment: 'waline'
---

### var 和 let 的区别：

var 是函数作用域，let 是块级作用域。

```javascript
function buildUrl() { 
    let qs = "?debug=true"; 
    with(location){ //在严格模式下，with 不允许使用
        let url = href + qs; 
    } 
    return url; 
} 
var getEle = buildUrl();
console.log(getEle);// 报错，url is not defined 第 7 行
```

因为 let 在 with 中使用，其作用域也仅限于 with 所拥有的花括号中。因此，用 let 声明的 url 在 with 外边 找不到。

```javascript
function buildUrl() { 
    let qs = "?debug=true"; 
    with(location){ 
        var url = href + qs; 
    } 
    return url; 
} 
var getEle = buildUrl();
console.log(getEle);//正确输出文件位置
```

我们先简单分析一下作用域链:

![img](https://cdn.nlark.com/yuque/0/2022/jpeg/26688609/1663246460012-639aecc3-0de2-46fc-bd13-4ee825031be1.jpeg)

如上图，下级可以访问上级作用域属性，但是上级不可以向下访问。因此代码中的 href 实际上是 **location.href,** 其中的qs也是函数中的。使用 let 声明时，url 只会被加入with 的作用域中。外部访问不到。但当使用 var 声明时，这个 url 是会在 函数初始化时。加入函数的作用域中。因此，在函数中，能访问到。



### 变量声明：

1. var 在声明的时候，会被自动添加到最接近的上下文。在函数中，最接近的上下文就是函数的局部上下文。在 with 语句中，最接近的上下文也是函数上下文。**如果变量未经声明就被初始化了，那么它就会自动被添加到全局上下文。**

```javascript
function add(num1,num2) {
    var num = num1 + num2;
    return num;
}
let res = add(10,20);
console.log(res);//30
console.log(num);//报错

//	不使用 var 声明 num 
function add(num1,num2) {
    num = num1 + num2;
    return num;
}
let res = add(10,20);
console.log(res);//30
console.log(num);//30
```

注意：未声明就初始化变量，是一个错误。在严格模式下会报错。

1. let 是块级作用域。在一个块中，如果 用 let 声明两次，则会报错，但是用 var ，重复的声明会被忽略。
