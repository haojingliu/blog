---
layout: post
title: 你不知道的JS Part I 作用域和闭包
comments: true
category: javascript
permalink: /:year/:month/:day/:title
tag: misc, javascript
---

这套书早就想撸完的，奈何一直偷懒。趁着2018的尾巴，还是赶紧完成吧。

###第一章 作用域是什么
Compiling: Tokenizing->Parsing->Code Generation

LHS：对变量进行赋值 不成功：隐式创建变量(unstrict mode)或ReferenceError异常
RHS：获取变量值 不成功：ReferenceError异常

###第二章 词法作用域
可以用eval()和with()来欺骗作用域。缺点是引擎无法在compiling的时候优化，所以会导致代码运行变慢。

eval和with在strict模式下被禁止了==。

###第三章 函数作用域和块作用域
函数的作用域：属于这个函数的全部变量都可以在整个函数的范围内使用及复用（在嵌套函数的作用域中也可以使用）。

IIFE(Immediately Invoked Function Expression)的两种形式：
(function() {}())和(function(){})()

ES3后try catch中的catch分句有块作用域

ES6后才增加了块作用域{}
###第四章 提升
对js来说，var a=2会解读成var a（编译阶段）; a = 2(原地等待执行)。只有声明本身会提升。

函数声明会被提到普通的变量声明之前。

###第五章 作用域闭包

当函数可以记住并访问所在词法作用域时，就产生了闭包。（只要使用了回调函数，就是使用了闭包）

模块模式特点：1）调用了包装了函数定义的外部的包装函数 2）封装函数返回至少一个内部函数

ES6中，将文件当做独立的模块来处理。import可以导入整个api或单独一个api。export会讲当前模块一个标志符（variable or function）导出为public API。

词法作用域是在定义时确定的【何处声明】；动态作用域是在运行时确定的【何处调用】（如this）。
