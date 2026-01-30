---
title: butterfly主题使用twikoo再vercel上部署的评论系统
date: 2023-09-13 19:12:33
author:
cover: '../../pic/twikoo.jpg'
tags:
- 个人博客搭建
categories:
---

# 前言

第一次搞博客，总想着试试新功能，这次就来准备搞搞这个评论系统， 通过Twikoo + Vercel 来进行部署，在我研究的时候也是看着这个[操作文档]([云函数部署 | Twikoo 文档](https://twikoo.js.org/backend.html#vercel-部署))慢慢搞的

# 申请MongoDB账号

1. 打开[注册页面](https://www.mongodb.com/cloud/atlas/register)

2. 填写个人信息

3. 创建免费 MongoDB 数据库，区域推荐选择 `AWS / N. Virginia (us-east-1)`也可以选择HONG KANG 其实 但是后面在Vercel 配置的时候也需要修改,怕出其他变数 所以这里只推荐这个区域了

4. 然后 `new a project` 创建一个新项目 在这个项目里 再` create a deploment`

   ![image-20230913192230967](../../pic/image-20230913192230967.png)

5. 选择好 `免费套餐` ![image-20230913192305617](../../pic/image-20230913192305617.png)

6. provider 选择 aws 亚马逊的 区域就是上面说过的 N. Virginia(us-east-1)

7. name 随便写都可以 写完之后就可以`create`了

8. 创建之后会进入一个新页面 将会开始创建你的 数据库了

9. ![image-20230913192845923](../../pic/image-20230913192845923.png)

   这里的username是你数据库的名字 password就是你数据库的密码 密码账号一定要牢记（后面还有用

   写完之后就`create user`即可

10. 然后往下滑就会出现一个 可以访问的IP地址 这里需要你添加一个0.0.0.0\0 意思是谁都可以访问。

11. 设置好之后 就Finishi and Close 这个时候你的数据库就创建好了

12. 然后就会自动跳转到 项目列表里 你的数据库就在那里加载着 ![image-20230913193141206](../../pic/image-20230913193141206.png)

    这个时候你点击connect 选择第一个![image-20230913193204955](../../pic/image-20230913193204955.png)

    ![image-20230913193226636](../../pic/image-20230913193226636.png)

    选择好你对应的node版本 然后将你的下面的MongoDB的字符串复制一下  然后并且将其中的`<PASSWORD>`给替换成你刚刚设置好的数据库密码 然后保存下来（后面还有用！）

13. 这个时候 Twikoo 基本上就配置完毕了

# Vercel

1. 用github 登录vercel

2. 然后使用github去创建一个仓库与Vercel进行一个链接![image-20230913193618071](../../pic/image-20230913193618071.png)

   连接成功之后会自动跳转页面并且弹出礼花![image-20230913193650079](../../pic/image-20230913193650079.png)

   然后点击右上角的`continue to Dashboard`  就会跳转页面

   ![image-20230913193754096](../../pic/image-20230913193754096.png)

   然后点击Settings—》Environment Variables 填写对应的 key 和value 这里的key就是`MONGODB_URI` value就是刚刚从MongoDB里复制下来的并且将密码替换后的字符串

3. 回到project 之中 然后点击Domains中 添加你的域名 需要进行DNS解析 国内才可以正常访问

4. 解析地址一般是76.76.21.21 国内用  76.223.126.88 感觉会更快一点

5. 解析成功之后 你的project页面中 左侧图片就会说 云函数配置正常，你也可以正常且直接通过域名进行访问了

# 前端配置

我使用的是butterfly主题 在主题的配置文件里 可以进行直接配置,

```
comments:  # 这里是评论
  use: Twikoo
  text: true # Display the comment name next to the button
  lazyload: true  #这个是懒加载 只有当访客滑动到这个图片是才加载 不开启会降低访问速度
  count: false # Display comment count in post's top_img
  card_post_count: false # Display comment count in Home Page
```

```y
# 这里再进行对twikoo的配置
twikoo:
  envId: # 这里写你绑定的域名
  region:
  visitor: false
  option:
```

配置下来使用评论系统了！

# 视频

这里是视频 相较于文字 应该会更好的理解吧

<iframe src="//player.bilibili.com/player.html?aid=788420683&bvid=BV1414y1r7bS&cid=1266356182&p=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
