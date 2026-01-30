---
title: 关于在hexo上部署百度统计的操作
date: 2023-01-07 11:29:14
tags:
- 个人博客搭建
---

##### 不得不说一句的是 我目前是一名大学生 在大学中也是一直混日子 老师什么都没有教过这方面的知识 仅仅是自学自我理解 因而所以我将会记录这个博客搭建的每一个详细步骤

## 要求

百度账号
用户名
账号
密码
手机号

## 步骤

1. 搜索百度统计

2. 登录百度账号

3. 在使用设置中点击新增网页

   ```
   网站域名: https://watch-with-tenderness.gitee.io/tenderness/
   网站首页: https://watch-with-tenderness.gitee.io/tenderness/
   网站名称: 温柔目送
   行业类别: 自媒体
   ```

   这是我的相关填写回应

4. 填写完成之后 会得到一串 script 代码片段

   ```
   <script>
   var _hmt = _hmt || [];
   (function() {
     var hm = document.createElement("script");
     hm.src = "https://hm.baidu.com/hm.js?3db2892b47d3a66ec20c33f848842d2b";
     var s = document.getElementsByTagName("script")[0]; 
     s.parentNode.insertBefore(hm, s);
   })();
   </script>
   ```

5. 将这个片段填写在 hexo根目录下主题 theme 中 你选用的主题 中的 layout\footer.ejs

6. 填写完成之后记得 hexo d 重新推送到gitee 并且进行重新部署

7. 然后再回到 百度统计中 使用设置中 查看报告旁边 刷新一下 检查一下代码是否安装正确 即可
