---
title: freebuf报告的数据爬取
date: 2023-09-13 19:51:42
author: 温柔目送
cover: '../../../pic/爬虫.png'
tags:
- 爬虫
categories:
- 爬虫
- 实战
---
# 前言
代码具有时效性，该页面随时可能会增加反爬机制，or页面框架被修改，而导致改代码无法正常获取数据 是正常的现象

# 代码


```python


import requests
from lxml import etree
import json
import time


#  先获取所有需要的链接地址
for page in  range(24):
    url = f'https://www.freebuf.com/fapi/frontend/category/list?name=paper&tag=category&limit=20&page={page}&select=0&order=0'
    head_url = 'https://www.freebuf.com/articles/paper'
    headers ={
        'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36 Edg/116.0.1938.76'
    }
    resonpe = requests.get(url,headers=headers)
    xx = resonpe.json()
    for i in xx['data']['data_list'] :
        page = i['url'].split('/')[-1]
        son_page = head_url + '/' + page
        son_url = requests.get(son_page,headers)
        # 子页面源代码已经获取成功
        son_url.encoding = 'utf-8'
        # print(son_url.text)
        tree = etree.HTML(son_url.text)
        #     拿到了文章名
        title = tree.xpath('//*[@id="artical-detail-page"]/div[3]/div[2]/div[1]/div[1]/div[1]/span/text()')
        article = tree.xpath('//*[@id="tinymce-editor"]/div')
        for i in article:
            p_text = i.xpath('./p/text()')
            print('已经进行到文章页面的部分 准备进行内容获取')
            for text1 in p_text:
                with open(f'./文章存储/{title[0]}.txt' ,mode='a',encoding='utf-8') as f:
                    f.write(f"{text1}\n")
                    print('p已经保存完毕')
                    time.sleep(2)
            li_text = i.xpath('./ul/li/text()')
            for text2 in p_text:
                with open('./文章存储/' + title[0], mode='a', encoding='utf-8') as f:
                    f.write(text2)
                    print('小li已经保存完毕')   ######
                    time.sleep(2)

            h2_text = i.xpath('./h2/text()')
            for text3 in p_text:
                with open('./文章存储/' + title[0], mode='a', encoding='utf-8') as f:
                    f.write(text3)
                    print('H2标题已经保存完毕')
                    time.sleep(2)

            h3_text = i.xpath('./h3/text()')
            for text4 in p_text:
                with open('./文章存储/' + title[0], mode='a',encoding='utf-8') as f:
                    f.write(text4)
                    print('H3文章已经保存完毕')
                    time.sleep(2)
    print('ALL ok')
    #   拿到当前文章的所有内容了
    resonpe.close()
	
```

# 不足

其实在写这个代码的时候 还是用我的老习惯了属于是 直接硬生生的写，不像是其他人的 去使用一个函数慢慢的来进行搭建，我这样写，确实不够干净整洁 模块也分的不是很清楚（其实都没分）

实际效果

![image-20230913195553108](../pic/image-20230913195553108.png)

成功的爬取到了多篇文章，且由于增加了 time.sleep 睡眠机制成功的躲避了服务器对IP的封查，这样做也就代表了我可以爬取更多的数据
