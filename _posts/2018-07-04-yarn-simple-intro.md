---
layout: post
title: Yarn的小介绍
comments: true
category: misc
permalink: /:year/:month/:day/:title
tag: misc, tech
---

最近做的项目的参考的repo里用到了yarn这个技术，所以来记录下一些简单的小知识。

这里说的yarn是试图替代npm的一个node的包管理器，不是Hadoop里的yarn，虽然他们名字都是Yet Another Resource Negotiator。

Yarn是由Facebook、Google、Exponent 和 Tilde 联合推出了一个新的JS包管理工具。在编写代码中经常发现如果用了npm install，即使想装三四个简单的依赖，由于依赖的依赖，实际上装的包都会变成十几个。yarn能精简真正拉下来的依赖的数量，不过yarn拉package依然从npm的repo里拉的

yarn有如下这么几个小特性：

---
yarn的[安装](https://yarnpkg.com/en/docs/install)不依赖于node。

yarn像composer一样，有yarn.lock文件，用于track当前安装的包的版本，这里的用法比npm好很多，只要当前包里版本发生变化就能用版本管理里看出来。因为指定安装版本的时候可以写成"1.0.3"（安装"1.0.3"版本），"^1.0.3"（安装1.X.X中最新的版本），"~1.0.3"（表示安装1.0.X中最新的版本），很容易错好不好！！！

看到有人测试说yarn install比npm install快很多，基本是三分之一的速度，不过有人也说如果用taobao镜像就会快许多，没有实验数据那么吓人

好多操作yarn的操作命令会更简洁些
```
npm install
npm run dev
```
VS
```
yarn
yarn start
```


######PS：
另外一个小知识我总是忘，要记录下，这个是yarn和npm都有的关于--save和--save-dev的区别
- --save: dependencies for application to run;
- --save-dev: dependencies for development like unit test, magnifications etc.