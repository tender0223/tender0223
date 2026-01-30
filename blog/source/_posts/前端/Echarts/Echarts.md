---
title: Echarts
date: 2023-03-19 09:37:14
tags:
- 前端
- Echarts
---

## [Echarts](https://www.echartsjs.com/zh/index.html)的学习

为什么要学习[Echarts ](https://www.echartsjs.com/zh/index.html)？

 因为要做出一个数据可视化 并且显示的比较有B格 需要用到表格

 同时需要将[Echarts ](https://www.echartsjs.com/zh/index.html)连接到服务器所以我们需要学习一下

### 使用

引入[echarts.js](https://watch-with-tenderness.gitee.io/2023/03/19/Echarts/..\images\echarts-5.4.1.zip) 文件

```JS
<script src="echarts.js"></script>
```

### 我们准备好一个DOM容器

````HTML
<!DOCTYPE html>
<html>
  <head>
    <title>ECharts</title>
  </head>
  <body>
    <div id="main" style="width: 600px;height:400px;"></div>
  </body>
</html>
````

### 初始化 echarts 实例对象

```JS
  <script type="text/javascript">
      // 基于准备好的dom，初始化echarts实例
      var myChart = echarts.init(document.getElementById('main'));
    </script>
```

### 指定配置项和数据

```JS
// 指定图表的配置项和数据
var option = {
    title: {
        text: 'ECharts 入门示例'
    },
    tooltip: {},
    legend: {
        data: ['销量']
    },
    xAxis: {
        data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
    },
    yAxis: {},
    series: [
        {
            name: '销量',
            type: 'bar',
            data: [5, 20, 36, 10, 10, 20]
        }
    ]
};
```

### 将配置项设置给echarts 对象

```JS
    myChart.setOption(option);
```

# 整个流程

```HTML
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>ECharts</title>
    <!-- 引入刚刚下载的 ECharts 文件 -->
    <script src="echarts.js"></script>
  </head>
  <body>
    <!-- 为 ECharts 准备一个定义了宽高的 DOM -->
    <div id="main" style="width: 600px;height:400px;"></div>
    <script type="text/javascript">
      // 基于准备好的dom，初始化echarts实例
      var myChart = echarts.init(document.getElementById('main'));

      // 指定图表的配置项和数据
      var option = {
        title: {
          text: 'ECharts 入门示例'
        },
        tooltip: {},
        legend: {
          data: ['销量']
        },
        xAxis: {
          data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
        },
        yAxis: {},
        series: [
          {
            name: '销量',
            type: 'bar',
            data: [5, 20, 36, 10, 10, 20]
          }
        ]
      };

      // 使用刚指定的配置项和数据显示图表。
      myChart.setOption(option);
    </script>
  </body>
</html>
```

