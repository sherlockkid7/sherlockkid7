---
title: 数据可视化ECharts项目
date: 2020-06-08 18:27:00
tags: ECharts
categories: ECharts
---

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200608152310511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)
此项目是根据[pink老师的课程](https://www.bilibili.com/video/BV1v7411R7mp?p=51)实现的ECharts数据可视化项目
项目地址： [http://tq07.gitee.io/echarts](http://tq07.gitee.io/echarts)
### Echarts基础知识

> ECharts，一个使用 JavaScript 实现的开源可视化库，可以流畅的运行在 PC和移动设备上，兼容当前绝大部分浏览器（IE8/9/10/11，Chrome，Firefox，Safari等），底层依赖矢量图形库ZRender，提供直观，交互丰富，可高度个性化定制的数据可视化图表。
> 官网地址：<https://www.echartsjs.com/zh/index.html>

#### 1.使用步骤：

1.获取 ECharts （[Apache ECharts (incubating) 官网下载界面](https://echarts.apache.org/zh/download.html)）（该项目使用直接下载的ECharts插件）

2.引入ECharts 插件到html页面中

```js
<script src="./js/echarts.min.js"></script>
```

3.准备一个具备大小的DOM容器

```html
<body>
    <!-- 为 ECharts 准备一个具备大小（宽高）的 DOM -->
    <div id="main" style="width: 600px;height:400px;"></div>
</body>
```

4.初始化echarts实例对象

```js
// 基于准备好的dom，初始化echarts实例
 var myChart = echarts.init(document.getElementById('main'));
```

5.指定配置项和数据(option)

```js
var option = {
            title: {
                text: 'ECharts 入门示例'
            },
            tooltip: {},
            legend: {
                data:['销量']
            },
            xAxis: {
                data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
            },
            yAxis: {},
            series: [{
                name: '销量',
                type: 'bar',
                data: [5, 20, 36, 10, 10, 20]
            }]
        };

```

6.将配置项设置给echarts实例对象

```js
myChart.setOption(option);
```

#### 2.Echarts基础配置

- series（系列）
  每个系列通过 `series.type` 决定自己的图表类型（图表数据，指定什么类型的图表，可以多个图表重叠）。
- xAxis：直角坐标系 grid 中的 x 轴
- yAxis：直角坐标系 grid 中的 y 轴
- grid：直角坐标系内绘图网格 
- title：标题组件
- tooltip：提示框组件
- legend：图例组件
- color：调色盘颜色列表
- boundaryGap: 
   坐标轴两边留白策略，类目轴中 boundaryGap 可以配置为 true 和 false。默认为 true，可以保证刻度线和标签对齐。这时候刻度只是作为分隔线，标签和数据点都会在两个刻度之间的带(band)中间。![在这里插入图片描述](https://img-blog.csdnimg.cn/20200608161640464.png)
- 数据堆叠，同个类目轴上系列配置相同的`stack`值后 后一个系列的值会在前一个系列的值上相加。

示例：
```js
let option = {
			// color设置线条的颜色 注意后面是个数组
        	color: ['pink', 'red', 'green', 'skyblue'],
       		 // 设置图表的标题
		    title: {
		        text: '折线图堆叠123'
		    },
		    // 图表的提示框组件
		    tooltip: {
		    // 触发方式
            trigger: 'axis',
            // 坐标轴指示器，坐标轴触发有效
            axisPointer: {
                type: 'shadow'  // 默认为直线，可选为：'line' | 'shadow'
            }
        },
        // 图例组件
	    legend: {
	       // series里面有了 name值，则 legend里面的data可以删掉
	    },
	    // 网格配置  grid可以控制线形图 柱状图 图表大小
        grid: {
            top: "10%",
            left: "22%",
            bottom: "10%"，
            // 是否显示刻度标签 如果是true 就显示， false则不显示
           containLabel: true
        },
        // 工具箱组件  可以另存为图片等功能
	    toolbox: {
	        feature: {
	            saveAsImage: {}
	        }
	    },
	    // 设置x轴的相关配置
        xAxis: {
            show:false
        },
        // 设置y轴的相关配置
        yAxis: [
            {
                type: 'category',
                inverse: true,
                data: ["HTML5", "CSS3", "javascript", "VUE", "NODE"],
                //不显示y轴线条
                axisLine: {
                    	show: false
                    	// 如果想要设置单独的线条样式 
					    // lineStyle: {
					     	//    color: "rgba(255,255,255,.1)",
					       //    width: 1,
					       //    type: "solid"
					      //  }
                        },
                // 不显示刻度
                axisTick: {
                show: false
                },
               Y轴的文字颜色和大小
                axisLabel:{
                    color:"#fff"，
                    fontSize: "12"
                }，
                // y 轴分隔线样式
			   splitLine: {
			       lineStyle: {
			          color: "rgba(255,255,255,.1)"
			        }
			   }
            },
            {
                type: 'category',
                inverse: true,
                data: [702, 350, 610, 793, 664],
                //不显示y轴线条
                axisLine: {
                    show: false
                        },
                // 不显示刻度
                axisTick: {
                show: false
                },
                
                axisLabel:{
                    color:"#fff",
                    fontSize:12
                }
            },
        ],
        // 系列图表配置 它决定着显示那种类型的图表
        series: [
            {
                name: 'bar',
                type: 'bar',
                // 柱子之间的距离
                barCategoryGap:50,
                // 修改柱子宽度
                barWidth:10,
                itemStyle:{
                		 // 修改柱子圆角
                        barBorderRadius:20,
                        //给 itemStyle  里面的color 属性设			置一个 返回值函数
                        // params 传进来的是柱子对象
                        color:function(params){
                        // dataIndex 是当前柱子的索引号
                        return myColor[params.dataIndex];
                        } 
                },
                data: [70, 34, 60, 78, 69],
                // 图形上的文本标签
                label:{
                    normal:{
                        show: true,
                        // 图形内显示
                        position: "inside",
                        // 文字的显示格式   {c} 会自动的解析为 数据  data里面的数据
                        formatter: "{c}%"
                    }
                },
                yAxisIndex: 0, //层级关系
            },
            {
                name: 'box',
                type: 'bar',
                barCategoryGap: 50,
                barWidth: 15,
                itemStyle: {
                    color: "none",
                    borderColor: "#00c1de",
                    borderWidth: 3,
                    barBorderRadius: 15
                },
                data: [100, 100, 100, 100, 100],
                yAxisIndex: 1, //层级关系
            }
        ]
    }; 
```
补充知识点：
1.折线类型图表把折线修饰为圆滑 ，在series 数据中添加 smooth 为 true
2.带有直角坐标系的比如折线图、柱状图是 grid修改图形大小，而饼形图是通过 radius 修改大小
3.饼图图形上的文本标签可以控制饼形图的文字的一些样式， label 对象中设置
```js
	// 文本标签控制饼形图文字的相关样式， 注意它是一个对象
        label: {
          fontSize: 10
        },
```

4.引导线调整，在series对象里面的  labelLine  对象设置
 	

```js
		// 引导线调整
        labelLine: {
         // 连接扇形图线长
          length: 6,
         // 连接文字线长
         length2: 8
        } 
```

5.地图放大通过  zoom ，设置为数字
### 项目使用的技术：
- div + css 

- flex 布局

- Less  （在html页面中需导入对应的css文件，在VScode中安装Easy LESS插件可以直接把less文件转成css文件）

- 原生js

   注意：每个图表指定配置项和数据(option)需要使用立即执行函数或者在函数中使用let声明变量，避免命名冲突

- rem适配
 
- 引用淘宝的flexible.js 把屏幕分为 24 等份
-  VScode中安装cssrem插件，并修改cssrem 插件的基准值为 80px 
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200608181816692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)注：别忘记重启vscode软件保证生效