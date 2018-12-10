---
layout: post
title: 你不知道的JS Part II this和对象原型
comments: true
category: javascript
permalink: /:year/:month/:day/:title
tag: misc, javascript
---

###第一、二章 this
常见错误理解：
X this指向函数自身
X this指向函数作用域

this是在运行时绑定，与声明无关，只取决于函数调用方法。

this绑定方法
默认绑定：在unstrict模式下绑定到全局对象；strict模式下绑定到undefined。
隐式绑定：当函数引用有上下文时，会绑定this到上下文对象（最后一层调用位置）。
显示绑定：使用call和apply；创建包裹函数；创建辅助函数；ES5内置方法Function.prototype.bind
new绑定

new的步骤：
1. 创建一个全新的对象
2. 新对象执行prototype连接
3. 新对象会绑定到this
4. 如果函数没有返回其他对象，那么new表达式的函数调用会自动返回新对象


可以用...代替apply(..)来展开数组

空对象 var emptyObj = new Object(null);

箭头函数的绑定this无法修改，所以常被应用于回调函数中。

### 第三章 对象
为什么typeof null是object，而null本身是一个基本类型?
因为js中二进制前三位为0为判定为object，null的二进制表示全为0

对象中的属性名永远是字符串。ES6中增加了可计算属性名：
```
var myObject = {
    [prefix + "bar"]: 1
}
```

从技术角度上说，函数永远不会"属于"一个对象。

ES5开始，所有属性都具备属性描述符（value, writable, enumerable可出现在对象属性遍历中, configurable）

##### "a" in myObject VS myObject.hasOwnProperty("a")

in操作符会检查对象和prototype原型链；hasOwnProperty只会检查是否在MyObject对象中。

##### 检查方法是否在object中
Object.prototype.hasOwnProperty.call(myObject, "a")

.propertyName本质上回调用[[Get]]操作，还会找到Prototypes链。

### 第四章 混合对象的类
javascript中的父类和子类的关系只存在于两者构造函数对应的prototype对象中

js的对象机制不会自动执行复制行为（子类复制父类），并不存在真正的类。js模拟其他语言的复制行为，使用混入（mixin）

显式混入：写个mixin(sourceObj, targetObj)
隐式混入：在子类的function中调用父类function。

js中模拟类是得不偿失的，因为无法完全模拟类的复制行为。对象/函数只能复制引用。

### 第五章 原型
所有的prototype链都会指向内置的Object.prototype。Object.prototyppe包含汗多通用功能.toString()、.valueOf等

myObj.foo = "bar"赋值时如果myObj没有foo就会向prototype链里面赋值。如果不想向上赋值，就只能用Object.defineProperty

当new使用时，函数调用会变成构造函数调用。

.constructor只是通过prototype调用了。constructor会随着prototype的改写而改写。

### 第六章 行为委托
[[prototype]]机制就是指对象中的一个内部链接引用另一个对象。

委托模式：使用不相同根据描述的方法名；避免使用伪多态调用（Widget.call, Widget.prototype.render.call）

匿名函数缺点
1. debug stack更难
2. 自我引用更难（递归等）
3. 代码更难理解