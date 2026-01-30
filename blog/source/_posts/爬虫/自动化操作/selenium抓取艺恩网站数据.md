---
title: selenium抓取艺恩网站数据
date: 2023-09-06 14:25:07
tags:

categories:
- 爬虫
- 自动化实战
cover:
---

 ```python
 from selenium.webdriver import Edge
 
 # 下拉列表
 from selenium.webdriver.support.select import Select
 from selenium.webdriver.common.keys import Keys
 import time
 
 web = Edge()
 web.get('https://www.endata.com.cn/BoxOffice/BO/Year/index.html')
 
 print(web.title)
 select = web.find_element_by_xpath('//*[@id="OptionDate"]')
 sel = Select(select)
 #操控浏览器 选择选项
 for i in range(len(sel.options)):
     #i 就是每一个option的索引值
     # print(i.text)
     sel.select_by_index(i)
     time.sleep(2)
     allpageText = web.find_element_by_xpath('//*[@id="TableList"]/table/tbody').text
     with open('无头浏览器内容',mode='a',encoding='utf-8') as f:
         f.write(allpageText)
     print('=================================================')
 
 print('ok')
 web.close()
 
 # 拿取页面原代码  是已经收到服务返回来内容的页面原代码 而不是纯原生的
 # print(web.page_source)
 
 ```

