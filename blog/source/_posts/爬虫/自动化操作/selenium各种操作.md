---
title: selenium各种操作
date: 2023-09-06 14:12:54
tags:
categories:
- 爬虫
- 自动化操作
cover:
---

# 对于selenium的前言

这个是我接触的第一个自动化的语言，我对他的理解，可以看作的针对于网页版的一个按键精灵，执行一些点击命令，对于可视化算是不错，相当于之前爬虫的可视化了

既然要演示各种操作 那我们这次就需要一个实例来进行说明了

# 下载包

```python
pip install selenium=4.1.1 ## 当前使用的是4.7.2版本的selenuim 会导致edge 打开之后 自动关闭所以这里选择版本回退 使用老版本
# 下载浏览器驱动： 这个必须要有 同你的代码放在一个文件夹里即可 要保持好浏览器驱动版本和你的浏览器版本一样
    # MicrosoftWebDriver.exe
```

# 导入包

```python
from selenium.webdriver import Edge #这里我用的是Edge 要是谷歌 就换成Chrome 火狐就是firefox
# 控制键盘的一些键盘案件
from selenium.webdriver.common.keys import Keys
import time
```

# 使用包

```python
#创建浏览器对象
web = Edge()
# 打开一个网址

web.get("http://www.lagou.com")
print(web.title)
# web.

# 某个元素 =>> 找到北京
el = web.find_element_by_xpath('//*[@id="changeCityBox"]/ul/li[1]/a')
# el = web.find_element(by='//*[@id="changeCityBox"]/ul/li[1]/a')
el.click() #点击事件

time.sleep(2) #selenium 操作太慢 让上一步执行完后 休息一下 免得输入不上内容 这个太慢是因为策略的原因 可以修改
# 找到输入框
    # 输入内容
inputpage = web.find_element_by_xpath('//*[@id="search_input"]').send_keys('python',Keys.ENTER)
# inputpage.click() #点击事件
    #检索（要么回车 要么点击 搜索）

```



