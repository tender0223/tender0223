---
title: hexo的搭建以及在gitee上的部署
date: 2023-01-08 11:29:14
tags:
- 个人博客搭建
---

##### 不得不说一句的是 我目前是一名大学生 在大学中也是一直混日子 老师什么都没有教过这方面的知识 仅仅是自学自我理解 因而所以我将会记录这个博客搭建的每一个详细步骤

## 关于我对于hexo的学习 以及成功搭建完毕，在gitee上成功部署的方法

# 首先是需要几款软件 安装也很简单 都是傻瓜式安装 路径随意 安装最新的即可

1. [git](https://git-scm.com/)
2. [node.js](https://nodejs.org/en/)
3. [vscode](https://code.visualstudio.com/)

安装好之后进行检测
在桌面上打开cmd

```
node -v
npm -v
git --version
```

有输出版本号就算安装成功

然后开始选择好你的hexo的本地路径 也就是在哪一个文件夹下安装找到好之后
![如果图片丢失请联系llizhxu789@163.com](D:\blog\source\_posts\PIC\enter.png)
![如果图片丢失请联系llizhxu789@163.com](D:\blog\source\_posts\PIC\cmd.png)

```
npm install hexo-cli -g
hexo init 你要创建的本地文件夹的名字 (后续用blog来统称)
cd blog
npm install
```

这个时候你就可以运行一下hexo 来检查一下看看是否已经安装成功

```
hexo s
```

然后接下来就是检查你gitee 是否有ssh密钥 和 邮箱是否已经绑定
ssh密钥的创建
首先去gitee上创建一个仓库 不要初始化 创建完毕之后 在回到你blog的根目录下打开cmd 输入
![如果图片丢失请联系llizhxu789@163.com](D:\blog\source\_posts\PIC\2023.1.8-全局.png)

```
git config --global user.name "uname"
git config --global user.email "uemail"
ssh-keygen -t rsa -C "你绑定的gitee邮箱"
```

然后接下来就会获取到ssh密钥 存放在你c盘用户下本台电脑的管理名称下的一个.ssh文件夹下里面后缀名为.pub的

用记事本打开后全选复制到你gitee用户设置中的ssh设置粘贴即可

然后在初始化你的仓库 在服务中的gitee pages 去更新你的https

这时候在回到你blog根目录下 打开cmd输入

```
npm install hexo-deployer-git --save
```

在用vscode (我用的是vscode 其他可打开的软件也可以) 找到_config.yml 打开后修改

language: zh-CN
找到最下面的 deploy
type： git
rope： 填写你的gitee仓库代码里面的下载(clone/克隆) 复制里面的https粘贴到这里来
branch ： master
修改yml里面的url改成你刚刚在gitee pages中的网址

然后再回到blog根目录下 打开cmd
输入

```
hexo cl  //清理缓存
hexo g  //本地存储
hexo s  //本地渲染
hexo d  //git 推送
```

# 每次修改你的博客之后呢 都请一定注意 要去你这个gitee仓库里 的gitee pages 上更新的你网址 这样内容才会随着你的修改而不断更新

[这是视频地址](https://www.bilibili.com/video/BV19W4y1G7eh/?zw)上插入视频/)
