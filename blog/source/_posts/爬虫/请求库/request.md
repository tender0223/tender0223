---
title: 爬虫的学习01-request
date: 2023-09-03 10:50:24
tags:
categories:
- 爬虫
- 请求的发送
cover:
---

# 前言

我们必须要知道一个关键性的知识点——request有什么用？ 要request来干什么？

request是一个用来发送请求数据的一个小插件包，我们可以用它来进行请求回服务器端的数据且拿到

值得注意的是,request是可以拿到数据的,但是如果想要拿到html中的数据则需要注意*该数据是否是被写死的* 如果是写死,则直接可以用该页面的url,如果不是写死的则就需要使用 network 看看数据到底是从哪里来的 然后再写入那个的url地址

# 实测

首先我们需要先引入request的包如果没有则需要下载

```
npm install request
```



request是向服务器发送请求的数据，并且接收到返回的数据的一个插件

那么我们首先就需要先在自己的代码中导入自己的包 并且发送数据 看看结果如何

```python
# 导包
from request
# 准备好 url 和 请求头headers
# 开始发送数据
response = request.get(url,headers)
print(reesponse)  # RESPONSE[200]
```

这里的200 就代表已经发送并且成功收到了数据

如果想要看具体的浏览器内容则换成

```python
print(response.text)
```

则就会出具体的doc源代码页面了

这个时候也就是完成了一次 发送且接受了 我们想要的数据也就在这里面了

