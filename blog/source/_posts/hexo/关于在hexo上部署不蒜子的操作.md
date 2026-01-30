---
title: 关于在hexo上部署不蒜子的操作
date: 2023-01-08 11:29:14
tags:
- 个人博客搭建
---

##### 不得不说一句的是 我目前是一名大学生 在大学中也是一直混日子 老师什么都没有教过这方面的知识 仅仅是自学自我理解 因而所以我将会记录这个博客搭建的每一个详细步骤

## 不蒜子网址

```
http://busuanzi.ibruce.info/
```

不蒜子的步骤比较简单 但是我在部署的时候 又有一些不理解的地方 所以特来写一下这个帖子(? 这算是帖子么)

```
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```

## (怕看不见所以用标题来加粗字体) 这两行的代码的区别就是在于 结尾是用的pv 和 uv

## uv的方式，单个用户连续点击n篇文章，只记录1次访客数。

## pv的方式，单个用户连续点击n篇文章，记录n次访问量。

## 这俩都是记住一个站点的全部访问量 因而只能够在一个站点内部署一次那我就将这两个访问量 部署在总页面

```
<span id="busuanzi_container_site_uv">本站访客数<span id="busuanzi_value_site_uv"></span>人次</span>
<span id="busuanzi_container_site_pv">本帖子总访问量<span id="busuanzi_value_site_pv"></span>次</span>
```

// 一些小bug,那就是在部署这个不蒜子的 script 的时候如果你是在本地打开没有推送到gitee上之前 显示的访问量 是不蒜子的网页访问量 等你部署到gitee上之后 就会变成你的网页的访问量了
