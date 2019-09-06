---
layout:     post
title:      EChartsJS记录
subtitle:   EChartsJS记录
date:       2019-09-06
author:     BY
header-img: img/home-bg.jpg
catalog: true
tags:
    - EChartsJS
---
### 隐藏保存图片按钮
```switf
toolbox: {
                feature: {
                    // saveAsImage: {}
                }
            },
```

### 将 tooltip 框限制在图表的区域内
tooltip.confine boolean`[ default: false ]`
```switf
tooltip : {
                trigger: 'axis',
                axisPointer : {            // 坐标轴指示器，坐标轴触发有效
                    type : 'shadow'        // 默认为直线，可选为：'line' | 'shadow'
                },
                confine:true,
            },
```