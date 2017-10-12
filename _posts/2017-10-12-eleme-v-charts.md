---
layout:     post
title:      "v-charts图标"
subtitle:   "vue v-charts"
date:       2017-10-12
author:     "handsameliu"
header-img: "img/post-bg-vue.jpg"
tags:
    - vue
    - v-charts
---

> 由于项目需要，用到了图表，虽然echarts已经有vue版了，但还是略复杂，正好找到了饿了么的组件库，所以记录一下，绝对是`极其` `极其` `极其` 简单的一个图标封装插件。只要数据格式正确，秒出图表

 —— [V-Charts](https://github.com/ElemeFE/v-charts)
 —— [文档](https://elemefe.github.io/v-charts/#/)

<!-- more -->

### 安装

```
npm i v-charts -S
```
-------------------

    我所使用的是单页性质的，所以选择按需安装，也可以统一加入到vue实例中。 下面以折线图为例。具体可以到官网上找，文档也非常简洁明了

```javascript
<template>
    <div>
        <!-- 单条折线图 -->
        <ve-line :data="lineChartData" ></ve-line>
        <!-- 多条折线图 -->
        <ve-line :data="dbLineChartData" :settings="chartSettings"></ve-line>
    </div>
</template>
<script>

    import VeLine from 'v-charts/lib/line';             // 折线图

    export default {
        name: 'line',
        data(){
            return {
                chartSettings:{
                    // 图表设置
                    // area: true, // 线条堆叠面积背景色
                    // labelMap: { // 替换展示名称,需要和数据中的key一一对应
                    //     hei: '高压',
                    //     low: '低压'
                    // },
                    // metrics: ['hei', 'low'], // 展示的指标
                    // dimension: ['日期'],      // 展示x轴数据的维度
                    // label: { // 在xy轴交汇节点上直接显示数值
                    //     normal: {
                    //         show: true
                    //     }
                    // }
                },
                lineChartData:{ // 单条折线图
                    columns: ['日期', '销售量'],
                    rows: [
                        { '日期': '1月1日', '销售量': 123 },
                        { '日期': '1月2日', '销售量': 1223 },
                        { '日期': '1月3日', '销售量': 2123 },
                        { '日期': '1月4日', '销售量': 4123 },
                        { '日期': '1月5日', '销售量': 3123 },
                        { '日期': '1月6日', '销售量': 7123 },
                        { '日期': '1月7日', '销售量': 7124 },
                        { '日期': '1月8日', '销售量': 7125 },
                        { '日期': '1月9日', '销售量': 7126 },
                        { '日期': '1月10日', '销售量': 8123 },
                        { '日期': '1月11日', '销售量': 9123 },
                        { '日期': '1月12日', '销售量': 10123 },
                        { '日期': '1月13日', '销售量': 11123 },
                        { '日期': '1月14日', '销售量': 12123 },
                        { '日期': '1月15日', '销售量': 1123 },
                        { '日期': '1月16日', '销售量': 37123 },
                    ]
                },
                dbLineChartData:{
                    columns:['日期','hei','low',],
                    rows:[
                        // 只要键值对对上即可，内容是啥不重要
                        {'日期':'2017年10月11日14:41:24','hei':'140','low':'96'},
                        {'日期':'2017年10月12日14:41:24','hei':'120','low':'126'},
                        {'日期':'2017年10月13日14:41:24','hei':'110','low':'86'},
                        {'日期':'2017年10月14日14:41:24','hei':'160','low':'116'},
                        {'日期':'2017年10月15日14:41:24','hei':'120','low':'76'},
                        {'日期':'2017年10月16日14:41:24','hei':'130','low':'66'},
                    ]
                },
            }
        },
        components: { VeLine },
    }
</script>
<style scoped>

</style>
```


