---
title: absolute案例演示
date: 2023-01-21 11:29:14
categories:
- 前端
- 定位
tags:
- 前端
---

# 复制粘贴至html文件中即可使用

```
<!DOCTYPE html>
<html lang="zh-CN">
<style>
    * {
        padding: 0;
        margin: 0;
    }
    div {
        width: 100px;
        height: 120px;
    }
    .box1 {
        position: relative;
        top: 0;
        left: 80px;
        background-color: #4c4c4c;

    }
    .body {
        background-color: black;
        width: 500px;
        height: 500px;
        position: relative;
    }
    .box2 {
        background-color: blue;
        position: absolute;
        left: 100%;
        top: 100%;
    }
</style>
<body>
    <div class="body">
        <div class="box2"></div>
    </div>
    <div class="box1"></div>
</body>
</html>
```

## 代码解释

```
* {
    padding: 0;
    margin: 0;
}
```

- 是代表向当前html文件下的所有样式进行一个修改 这里的所有样式 包括但不限于(body div ul ol img audio vedio input……)

  padding 是设了内边距 为0

  margin 是设了外边距 为0

  ```
  div {
      width: 100px;
      height: 120px;
  }
  ```

  这个div是指 当前html内容下的所有 div标签属性

  width(宽度) 为100px

  height(高度) 为120px

  px 就是像素的意思

```
.box1 {
    position: relative;
    top: 0;
    left: 80px;
    # right: 80%
    background-color: #4c4c4c;
}
```

.box1 是引用了内容下的 calss=’box1’ 这个标签
类名需要通过 “.calss”来进行引用
relative 定位是相对定位
top 距离顶部的距离为0
left 距离左边80px
right 距离右边80% (这个百分值具体是多少 是看你长辈级的定位的宽度
EG： 一个长辈的div 宽度是1000px 他的儿子距离右边为80% 就是说 他距离右边800px)
设置了这个box1的颜色 为 #4c4c4c

这里特别解释一下

蓝色方框 类名是box2
在css样式引用中 通过 “.” 来进行引用
我给 box2 进行了一个 绝对定位

但是 box2的父亲 是div.body这个盒子呢 被我写了一个相对定位 距离顶部和左边的距离是100% 但是却没有根据浏览器width和height来进行计算

而是根据父亲的长宽来进行计算的
由此我们可以了解到一件事情 ralative定位是根据最近的长辈级的定位的宽度来进行计算

假设box2的父亲(div.body)没有任何定位 则box2这个盒子会根据当前页面的长宽来计算

而这个box1 的长辈级的定位 就只有body 因而会显示在距离top为0px left为80px的距离
