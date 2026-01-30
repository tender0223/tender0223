---
title: 爬虫的学习02——xpath
date: 2023-09-03 23:11:59
tags:
categories:
- 爬虫
- 信息内容的获取方式-xpath
cover:
---

# 前言

在这部分其实方式是有很多种的，比如说什么 re正则，本节要将的xpath，比如还有bs4等等等等可以说是很多种类，这三类我们后面都会挨个说到，但是写者想肯定还有更好跟多的没有获取方式

同时本篇文章在设想的情况下是 数据被写死在html页面中，发送的是get请求 无参数，不设置url headers

# 下载包

```
npm install lxml
```

# 引包

```python
from lxml import etree
```

# 前戏

 这里是用来在引入包之后 需要做的一些事情 比如设置好一些url 请求头 get还是post请求之类的 

这里的url headers 就不设值了 也使用get请求，因为不需要传输数据，只是单纯的请求一下

```python
import request
url = 'XXX'
headers = {'XXX':'XXXX'}
response = request.get(url,headers)
print(response) #RESPONSE[200]
```

# 使用

xpath这个方式是一个通过lxml文档检索来获取所需资源的方式

我们在使用的时候需要 引入lxml这个包中的etree组件在该组件中有个xpath的函数

所以我们在使用的时候应该是 先给etree 进行一个赋予

```python
tree = etree.HTML(response.text) # 这里是将获取到的参数（页面源代码）传递给etree参数 并且采用HTML的方式去解读收到的参数
xpath = tree.xpath('XXX/a/@href') #/@href 这里前缀必须是a 之类的具有一定超连接意义的 也可以是video img （不过img的是src）这里你也可以修改成 text去获取这个元素的文本内容
print(xpath) # 这里输出的就是a链接里面的href值了 （如果是text 那就是文本内容

```

如此这般就算使用成功了



# 结尾

最后只需要在将你获取的东西进行一个保存 然后关闭运行代码即可

```python
with open (xpath,mode='w') as f :
    f.write(xpath.content) # 获取到的xpath的东西输入进去
    print('ok') # 一个完成的提示符
response.close() # 关闭运行代码
```

