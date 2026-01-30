---
title: re的实战—豆瓣TOP250
date: 2023-09-06 13:51:16
tags:
categories:
- 爬虫
- 实战
cover:
---

```python
\#拿到页面原代码
\#用正则提取有效信息

import requests
import re
import  csv
url = 'https://movie.douban.com/top250'
header = {
"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36 Edg/114.0.1823.58"
}

response = requests.get(url,headers=header)
response.decode = 'GBK'
page_content = response.text

print(page_content)
\#解析数据
obj = re.compile(r'<li>.*?<div class="item">.*?<span class="title">(?P<title>.*?)</span>.*?<p class="">.*?<br>(?P<years>.*?)&nbsp.*?'
                 r'<span class="rating_num" property="v:average">(?P<star>.*?)</span>.*?'
                 r'<span>(?P<num>.*?)人评价</span>',re.S)
list = obj.finditer(page_content)

f = open("data.csv", mode="w")
csvwriter = csv.writer(f)
for i in list:
    print(i.group('title'))
    print(i.group('years').strip())
    print(i.group('star'))
    print(i.group('num')+'人评价')
    dic = i.groupdict()
    dic["years"] = dic['years'].strip()
    csvwriter.writerow(dic.values())
f.close()
print('over')
```

