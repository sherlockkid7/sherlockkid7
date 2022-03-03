---
title: vue项目（仿外卖APP）
date: 2020-05-27 22:46:49
index_img: /img/vue.png
tags: [vue]
categories: Vue
---

**项目:关于外卖业务的前后台分离Web App**

- 前台应用技术架构为: vue + vuex + vue-router + webpack + ES6； 
- 核心功能模块:商家, 商品, 购物车, 评论,用户等多个子模块;
- 采用模块化、组件化、工程化的模式开发；
- 后台使用 mockjs 模拟后台数据接口和API接口;

**项目展示地址([点击前往](http://tq07.gitee.io/vue-food))**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430112702320.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430112702289.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430112702290.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430112702280.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430112702228.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430112702136.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70)



**学习心得：**
1) 熟悉一个项目的开发流程 
2) 学会组件化、模块化、工程化的开发模式 
3) 掌握使用 vue-cli 脚手架初始化 Vue.js 项目 
4) 学会模拟 json 后端数据，实现前后端分离开发 
5) 掌握一些项目优化技巧

**项目中使用的Vue 插件或第三方库**
- 使用 vue-router 开发单页应用
- 使用 axios/vue-resource 与后端进行数据交互
- 使用 vuex 管理应用组件状态 
- 使用 better-scroll/vue-scroller 实现页面滑动效果
- 使用 mint-ui 组件库构建界面 
- 使用 vue-lazyload 实现图片惰加载 
- 使用 mockjs 模拟后台数据接口

**样式和布局**
- 使用 stylus 编写模块化的 CSS 
- 使用 Vue.js 的过渡编写酷炫的交互动画 

### 项目中遇到的问题（难点）和项目优化方法
1.解决点击响应延时 0.3s 问题
原因：当用户一次点击屏幕之后，浏览器并不能立刻判断用户是否要进行双击缩放，还是想要进行单击操作。因此，iOS Safari 就等待 300 毫秒，以判断用户是否再次点击了屏幕。
解决方法：利用FastClick，其原理是检测到touchend事件后，立刻出发模拟click事件，并且把浏览器300毫秒之后真正出发的事件给阻断掉

```
<script src="https://as.alipayobjects.com/g/component/fastclick/1.0.6/fastclick.js"></script>
    <script>
      if('addEventListener' in document){
        document.addEventListener('DOMContentLoaded',function(){
          FastClick.attach(document.body);
        },false)
      }
      if(!window.Promise) { 
      document.writeln('<script src="https://as.alipayobjects.com/g/component/es6-promise/3.2.2/es6-promise.min.js" '+'>'+'<'+'/'+'script>'); 
      }
    </script>
```
2.后台应用

> 后台应用负责处理前台应用提交的请求, 并给前台应用返回 json 数据 
> 前台应用负责展现数据, 与用户交互, 与后台应用交互
> API接口  [githup上面找的API接口](https://github.com/bailicangdu/node-elm/blob/master/API.md)

3.封装 ajax 请求模块

```
// ajax 请求函数模块
import axios from 'axios'
export default function ajax(url='',data={},type='GET'){
    return new Promise(function(resolve, reject){
	        // 执行axios异步请求
	        let promise
           if (type === 'GET') { 
            // 准备 url query 参数数据 
            let dataStr = '' //数据拼接字符串 
            Object.keys(data).forEach(key => { 
                dataStr += key + '=' + data[key] + '&'
            })
            if (dataStr !== '') { 
            dataStr = dataStr.substring(0, dataStr.lastIndexOf('&')) 
            url = url + '?' + dataStr 
            }
            // 发送 get 请求 
            promise = axios.get(url) 
        } else { 
            // 发送 post 请求 
            promise = axios.post(url,data) 
        }
        promise.then(response => { 
            // 成功调用resolve()
            resolve(response) 
        }).catch(error => { 
            // 成功调用reject()
            reject(error) 
        }) 
    })
}
```
4.vuex 应用组件状态
vuex 的核心管理对象 **store 对象模块**
state模块：状态对象
mutation type 常量名称模块
mutations 模块 ：直接更新state的多个方法的对象
actions 模块：通过mutations间接更新state的多个方法的对象

```
//store 对象模块
import Vue from 'vue'
import Vuex from 'vuex'
import state from './state'
import mutations from './mutations' 
import actions from './actions'
import getters from './getters'
Vue.use(Vuex)

export default new Vuex.Store({
  state,
  mutations, 
  actions,
  getters,
})
```
5.模拟(mock)数据/接口
利用 mockjs 拦截 ajax 请求, 生成随机数据返回
[mockjs](http://mockjs.com/)

 6.ShopGoods 组件
 - 内部使用了另外 3 个组件
    a. ShopCart: 购物车组件 
    b. Cart: 购物车操作组件
    c. Food: 食品详情组件 
 - 使用第三方库 better-scroll: UI 滑动

 问题：Cart组件在添加食物的时候，第一次增加时, 没有 count，如果直接添加属性并赋值，新添加的属性没有数据劫持==>数据绑定==>更新了数据但界面不变 
 解决方法：Vue.set(food, 'count', 1) 给有数据绑定的对象添加指定属性名和值的属性(有绑定) 
7.项目优化/扩展
（1）缓存路由组件对象

```
<keep-alive> 
	<router-view /> 
</keep-alive>
```

好处: 复用路由组件对象, 复用路由组件获取的后台数据
（2）路由组件懒加载

```
// 返回路由组件的函数，只有执行该函数才会加载路由组件，这个函数在请求对应的路由路径时才会执行
const Home = () => import('../views/Home.vue')
const Order = () => import('../views/Order.vue')
const Search = () => import('../views/Search.vue')
const Profile = () => import('../views/Profile.vue')
```

8.打包文件分析与优化
1) vue 脚手架提供了一个用于可视化分析打包文件的包 webpack-bundle-analyzer 和配置
2) 启用打包可视化: npm run build  -- --report