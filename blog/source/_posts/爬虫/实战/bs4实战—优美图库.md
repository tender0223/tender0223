---
title: bs4的实战—优美图库
date: 2023-09-04 09:15:44
author: 温柔目送
tags:
- 爬虫
categories:
- 爬虫
- 实战
cover: '../../../pic/爬虫.png'
---

# 前言

在使用本篇文章的代码的时候，我们应该做到，url的实时更新，保证我们请求的目标是没有错误的，

# 代码

```python
import requests
from bs4 import BeautifulSoup
import  time # 引入的时间包 后面有个代码行是完成一次要休息1s 
# 拿到主页面地址
url = 'https://www.umei.cc/touxiangtupian/'
header = {
'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36 Edg/114.0.1823.67'
}
respondUrl = requests.get(url=url ,headers=header)
respondUrl.encoding = 'utf-8'
url_page = BeautifulSoup(respondUrl.text, 'html.parser') # 拿到页面原代码 选择了html的编译器

# 思路解析 这里这样写是因为 图片中 他把所有的图片统统写到了一个名叫 taotu-main的div盒子里，所以我只需要找到这一个div标签中的所有图片标签即可相当于是找到了所有的图片标签。
url_list = url_page.find("div", class_='taotu-main').find_all('a')
for a in url_list: # 因为有很多图片连接 这里我们进行一个循环遍历 拿到所有。
    urlGoSon = a.get('href').strip('/touxiangtupian/') # 经典裁切手法
    urlSon = url + urlGoSon #拿到子页面的一个
    # 拿 第二个页面的图片
    respondUrlSon = requests.get(urlSon,headers=header) # 这里请求了每一个图组的路径
    respondUrlSon.encoding='utf-8'
    #从子页面中拿到下载路径
    UrlSon_page = BeautifulSoup(respondUrlSon.text,'html.parser')
    urlSon_page = UrlSon_page.find('div',class_='tsmaincont-main-cont-txt').find_all('img')
    for img in urlSon_page:
        imgTrue = img.get('src')
        #下载图片
        # 第一步 请求这个图片的地址
        imgRespond = requests.get(imgTrue)
          #这里拿到的是字节
        img_name = imgTrue.split('/')[-1]  #拿到这个url最后的 / 以后的内容
        with open('img/'+img_name,mode='wb') as f:
            f.write(imgRespond.content)
        print('over',img_name)
        time.sleep(1) # 每执行一行此代码 即停止1s
print('allover')
respondUrl.close()
```

