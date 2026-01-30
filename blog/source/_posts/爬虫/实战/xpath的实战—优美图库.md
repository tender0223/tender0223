---
title: xpath的实战—优美图库
date: 2023-09-04 08:20:48
tags:
- 爬虫
categories:
- 爬虫
- 实战
cover: '../../../pic/爬虫.png'
author: 温柔目送
---

# 前言

在使用本篇代码的时候 你需要做到的是 保证url当前的实时正确 包的正确引用

```python
import requests 
from lxml import etree 
# ——————————————————————————————————————————————
url = 'https://www.umei.cc/touxiangtupian/' # 一定要保证好这段的url正确性
# ——————————————————————————————————————————————
header = {
'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36 Edg/114.0.1823.67'
} # 设置好请求头 可以用你的 也可以用我的
respond = requests.get(url,headers=header)
respond.encoding='utf-8'  #在获取到页面源代码的时候 进行UTF编译以免出现一些读取乱码的问题
#拿到主页面代码了
tree = etree.HTML(respond.text) # 重赋值给参数tree

# 解释——1.接下来的思路是获取许多张图片的连接，获取到里面的每一个img下的src的值，通过该值来进行对图片的下载
a_list = tree.xpath('/html/body/div[3]/div[3]/ul/li/a/@href') #拿到href 但是是很多张图片的href 我需要循环遍历出来每一个href值
for list in a_list: 
    newlist = list.replace('/touxiangtupian/','') # 将多余无用的路径内容给进行替换为空 
    # 拿到了子页面的url
    urlSon = url+newlist
    
        # 怕大家看不懂特意在这里写下思路： 因为写下的这个代码是我首先通过url进入到这个网站上，然后获取到里面每一个图组的href值，打开图组，里面又会有许多张图片 又要获取这很多张图片的href值才可以进行下载，在这里上面是获取到了那些图组的href值，我们还接下来要在图组的基础上拿到图组内所有图片的href值。 以下图组称为子页面
        
    respondSon = requests.get(urlSon,headers=header) # 对这些子页面的href值进行请求    
    #子页面的 页面源代码
    respondSon.encoding='utf-8'  #再次编码
    treeSon = etree.HTML(respondSon.text) #重新绑定新参数内容
    img_list = treeSon.xpath('//*[@id="tsmaincont"]/div[4]/div[2]/img/@src')[0] #拿到所有图片的img href值 这里的0 是在获取的时候变成了一个tuple数组元素了我通过[0]拿到数组内第一个元素，就可以了
    name = img_list.split('/')[-1] #路径的裁切
    urlres = requests.get(img_list) #拿到了真正的图片href值
    
    #开始保存！！！
    with open('IMGXpath/'+name,mode='wb') as f:
        f.write(urlres.content)
        print('ok')
print('allok')
respond.close() #停止运行

```

