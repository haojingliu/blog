---
layout: post
title: GraphQL的小介绍
comments: true
category: misc
permalink: /:year/:month/:day/:title
tag: misc, tech
---

GraphQL是Facebook推出的一个查询语言，是用任何其他语言实现的一个用于查询的抽象层。它的提出常常用来对比广泛采用的RESTful的问题的，REST一旦采用之后，就会发现很多现实中的问题很难全部用resource的情况进行处理，因此就会多后台的api。GraphQL的理念是把所有的查询终端全部放在/graphql这个路径上，然后通过不同的query来获取不同的数据。其实可以理解成把很多后台的活转移了一部分放到前台。Graph和React结合的比较好，可能都是因为Facebook家出的东西，我看有人已经用GraphQL+Apollo来代替Redux了。

解决的RESTful API的问题除了上述的数据定制多N个api外，还有
- 有时候前台不支持PUT和DELETE 
- 每个resource都有自己一组endpoint，管理维护都会比较混乱
- Overfetching和Underfetching的问题，即是拉到冗余数据和可能会需要第二次请求去拿数据（没有合适的api支持的时候）
- 有更改就需要更改后台，前台可操作的很少

graphql在国内好像一直都不流行，看了下大家普遍反应是大公司改不起，小公司没有资源改。粗粗使用了一下，有些感觉很像thrift

支持的基本类型：Int、Float、String、Boolean、ID

#####基本类型
+ Query: 用于查询，同步执行
```
query {
  allUser {
    id
  }
}
```
+ Mutation: 用于更新，依次执行
```
mutation {
  updateAge (id: 1, age: 22) {
    id
    name
    age
  }
}
```
+ Subscription: 监听事件

#####代码分配
+ Schema: 定义Type类型，不同的type名字merge起来会覆盖
+ Models: 写具体的query
+ Resolvers: 定义interface，用于外部function调用借口，包含逻辑，再此处调用models里的方法

#####graphql vs graphiql
+ /graphql 该路径是访问的总路径
+ /graphiql 使用于debug和开发使用，上线时应该disable掉

#####Apollo GraphQL
Apollo是统一支持GraphQL API的插件，可以同意统一放在现有的后台之上。
包括client、server和engine三大块。

Client(iOS，Andriod，web)  <==> Apollo Server <====>  后台DB

engine是用来做cache、performance tracking的，有docker版本支持


Chrome上也有插件可以方便的进行debug

######PS：
Hapi（happy）是一种framework，常常和express.js来进行比较

meteor.js建立在nodejs上统一server和client端的中间件，贯彻着DB everywhere的思想，虽然用h5跨平台是个优势，但是官方这个库高度依赖mongodb。
