---
title: vue项目——购物APP
date: 2020-06-27 21:31:32
tags: vue
categories: Vue
---

### 项目基本搭建工作

使用vue-cli v4.3.1脚手架搭建项目
`vue create my-project`

#### 1.1目录结构

src目录下新建的文件

- assets   引入的css和img文件
- common   一些单独的模块，如封装的防抖函数
- network     网络请求
- components -> common/content   子组件（封装的单独组件）
- store   vuex组件状态管理
- router  路由模块
- views   主要的四大模块

​    .editorconfig  该文件是src的同级目录，代码书写风格（规范）

```js
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
```

#### 1.2配置主要目录的别名

在vue.config.js中配置

```js
module.exports = {
  configureWebpack:{
    resolve:{
      // 配置别名
      alias:{
      	//@是src目录的别名
        'assets':'@/assets',
        'common':'@/common',
        'components':'@/components',
        'network':'@/network',
        'views':'@/views'
      }
    }
  }
 }
```

#### 1.3设置CSS初始化和全局样式

- [initialize.css](https://github.com/necolas/normalize.css/blob/master/normalize.css)
- base.css

#### 1.4 创建主要的四个模块(路由组件)

home   category   cart    profile

##### 安装、使用vue-router（创建项目的时候没有选择自动创建路由模块）

`npm install vue-router --save`

创建路由实例

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

const Home = () => import('views/home/Home') 
const Category = () => import('views/category/Category')
const Cart = () => import('views/cart/Cart')
const Profile = () => import('views/profile/Profile')
// 1.注入插件
Vue.use(VueRouter)

// 2.定义路由
const routes = [
  {
    path:'/',
    redirect:'/home'
  },
  {
    path: '/home',
    name: 'Home',
    component: Home
  },
  {
    path:'/category',
    name:'Category',
    component: Category
  },
  {
    path:'/cart',
    name:'Cart',
    component: Cart
  },
  {
    path:'/profile',
    name:'Profile',
    component: Profile
  },
]

// 3.创建router实例
const router = new VueRouter({
   routes,
   mode:'history'
})

// 4.导出router实例
export default router
```

挂载到Vue实例中(main.js入口文件)

在App.vue文件中使用

#### 1.4 底部tabbar的封装

- 封装Tabbar为单独的模块

  - 外部框架封装成一个组件

    ```html
    <template>
      <div id="tab-bar">
        <slot></slot>
      </div>
    </template>
    ```

  - 每一个底部切换按钮封装成组件

    ```html
    <template>
      <div id="tab-item" @click="itemClick">
        <div v-if="!isActive">
          <slot name="item-icon"></slot>
        </div>
        <div v-else>
          <slot name="item-icon-active"></slot>
        </div>
        <div :style="styleColor">
          <slot name="item-text"></slot>
        </div>
      </div>
    </template>
    ```

- **响应点击切换的设计**

- 封装完成后，该项目使用时对Tabbar重新包装

  ```html
  <template>
    <div id="main-tab">
      <tab-bar>
        <tab-bar-item path="/home">
          <img slot="item-icon" src="~assets/img/tabbar/home.svg" alt="">
          <img slot="item-icon-active" src="~assets/img/tabbar/home_active.svg" alt="">
          <div slot="item-text">首页</div>
        </tab-bar-item>
        <tab-bar-item path="/category">
          <img slot="item-icon" src="~assets/img/tabbar/category.svg" alt="">
          <img slot="item-icon-active" src="~assets/img/tabbar/category_active.svg" alt="">
          <div slot="item-text">分类</div>
        </tab-bar-item>
        <tab-bar-item path="/cart">
          <img slot="item-icon" src="~assets/img/tabbar/shopcart.svg" alt="">
          <img slot="item-icon-active" src="~assets/img/tabbar/shopcart_active.svg" alt="">
          <div slot="item-text">购物车</div>
        </tab-bar-item>
        <tab-bar-item path="/profile">
          <img slot="item-icon" src="~assets/img/tabbar/profile.svg" alt="">
          <img slot="item-icon-active" src="~assets/img/tabbar/profile_active.svg" alt="">
          <div slot="item-text">我的</div>
        </tab-bar-item>
      </tab-bar>
    </div>
  </template>
  ```

#### 1.5 axios的封装

该项目使用了三个api接口的数据，因此封装了三个请求模块，其中一个因为有跨域问题，在开发环境下使用了反向代理（在vue.config.js中配置）

- 创建axios实例
- 拦截响应（需要返回拦截的data数据）
- 根据传入的options发送请求，并且调用对应resolve和reject

### 首页开发

#### 2.1 navbar的封装和使用

- 封装navbar包含三个插槽：left、center、right
- 设置navbar相关的样式
- 使用navbar实现首页的导航栏

#### 2.2 请求首页数据

封装首页的数据请求

```js
import {request,requestone,requesttwo} from 'network/request'

export function getHomeMultidata(){
  return request({
    url:'/home/multidata'
  })
}

export function getHomeFloordata(){
  return requestone({
    url:'/api/public/v1/home/floordata'
  })
}

export function getHomeImagedata(params){
  return requesttwo({
    url:'/ip/image/v3/homepage/vertical',
    params:params
  })
}
```

#### 2.3 根据Vant封装HomeSwiper

注意：

1.需要安装插件babel-plugin-import

```bash
npm i babel-plugin-import -D
```

2.在 babel.config.js 中配置 

```js
plugins: [
    ['import', {
      libraryName: 'vant',
      libraryDirectory: 'es',
      style: true
    }, 'vant']
  ]
```

以上当时没有安装使用，导致轮播图一直有问题

3.该轮播图触摸滑动时，会自动跳转到图片的链接地址，在van-swipe标签中使用API（`:stop-propagation="false"`）,就可以阻止滑动事件触发

#### 2.4 封装HomeRecommend

传入recommends数据，进行展示

#### 2.5 封装HomeFeature

展示本地的一张图片

#### 2.6 封装HomeFloor

传入messages数据，进行展示

#### 2.7 封装PageImage

传入vertical数据，进行展示

#### 2.8 滚动模块的封装（Scroll）

- 安装better-scroll（使用html的模板： wrapper -> content -> 很多内容）

- 封装一个独立的组件，用于作为滚动组件：Scroll

- 组件内代码的封装：

  - 创建BetterScroll对象，并且传入DOM和选项

    * 监听滚动

      probeType: 0/1/2(手指滚动)/3(只要是滚动)

    * click：false

      content里面div不可以监听点击，所以一般需要设置为true

    * 上拉加载   pullUpLoad: true

    ```js
    // 1.better-scroll的初始化
    this.scroll = new BScroll(this.$refs.wrapper, {
        click:true,
        probeType:this.probetype,
        pullUpLoad: this.pullUpLoad,
        mouseWheel: true,//开启鼠标滚轮
        // disableMouse: false,//启用鼠标拖动
        // disableTouch: false//启用手指触摸
    })
    ```

    ```js
    // 1.better-scroll的初始化
    this.scroll = new BScroll(this.$refs.wrapper, {
        click:true,
        probeType:this.probetype,
        pullUpLoad: this.pullUpLoad,
        mouseWheel: true,//开启鼠标滚轮
        // disableMouse: false,//启用鼠标拖动
        // disableTouch: false//启用手指触摸
    })
    ```

  - 监听scroll事件，该事件会返回一个position

    ```js
    // 2.监听滚动的位置
    if(this.probetype === 2 || this.probetype === 3){
       this.scroll.on('scroll',(position) => {
       this.$emit('scroll',position)
    })
    }
    ```

  - 监听pullingUp事件，监听到该事件进行上拉加载更多

    ```js
    // 3.监听上拉事件
    if(this.pullUpLoad){
      this.scroll.on('pullingUp',() => {
      this.$emit('pullingUp')  
      })
    }
    ```

  - 封装刷新的方法：this.scroll.refresh()

  - 封装滚动的方法：this.scroll.scrollTo(x, y, time)

  - 封装完成刷新的方法：this.scroll.finishedPullUp

#### 2.9 返回顶部

- 封装BackTop组件
- 监听滚动，决定BackTop组件在什么数值下的显示和隐藏
- 监听BackTop的点击，点击时，调用scrollTo返回顶部

#### 2.10 首页开发中遇到的问题

1.home组件直接监听back-top组件的点击, 不可以直接监听，必须添加修饰.native

使用 `v-on` 的 `.native` 修饰符：可以在一个组件的根元素上直接监听一个原生事件

2.首页中可滚动区域的问题

Better-Scroll在决定有多少区域可以滚动时, 是根据scrollerHeight属性决定

* scrollerHeight属性是根据放Better-Scroll的content中的子组件的高度
* 但是我们的首页中, 刚开始在计算scrollerHeight属性时, 是没有将图片计算在内的
* 后来图片加载进来之后有了新的高度, 但是scrollerHeight属性并没有进行更新.
* 所以滚动出现了问题

解决方法

* 监听每一张图片是否加载完成, 只要有一张图片加载完成了, 执行一次refresh()
* 如何监听图片加载完成了?
  * 原生的js监听图片: img.onload = function() {}
  * Vue中监听: @load='方法'
* 调用scroll的refresh()

解决过程中的难点

- PageImage.vue组件中监听图片加载的事件传入到Home.vue中

  因为涉及到非父子组件的通信, 所以这里我们选择了**事件总线**

  - 在main.js中初始化 `bus`  

  ```js
  //注意，这种方式初始化的 bus 是一个 全局的事件总线 
  Vue.prototype.$bus = new Vue()
  ```

  - 发送消息

    this.$bus.$emit('事件名称', 参数)

  - 接收事件

    this.$bus.$on('事件名称', 回调函数(参数))


* refresh找不到的问题
  *  在Scroll.vue中, 调用this.scroll的方法之前, 判断this.scroll对象是否有值
  *  在mounted生命周期函数中使用 this.$refs.scroll而不是created中

* 对于refresh非常频繁的问题, 进行防抖操作
  * 防抖debounce/节流throttle
  * 防抖函数起作用的过程:
    * 如果我们直接执行refresh, 那么refresh函数会被执行30次.
    * 可以将refresh函数传入到debounce函数中, 生成一个新的函数.
    * 之后在调用非常频繁的时候, 就使用新生成的函数.
    * 而新生成的函数, 并不会非常频繁的调用, 如果下一次执行来的非常快, 那么会将上一次取消掉

```js
      debounce(func, delay) {
        let timer = null
        return function (...args) {
          if (timer) clearTimeout(timer)
          timer = setTimeout(() => {
            func.apply(this, args)
          }, delay)
        }
      },
```

3.使用better-scroll遇到的问题

页面初始化以后滑动事件失效，需要从新刷新页面才能启动功能

在网上查了好久，有博客指出是新版谷歌与better-scroll自身的兼容问题，并不影响项目的正常运转，只是在谷歌进行移动端测试时才会出现，还有在创建BetterScroll对象时，添加`mouseWheel: true,//开启鼠标滚轮`可以使用鼠标滚动

### 商品分类开发

#### 3.1封装导航栏CateHeader

使用封装过的navbar组件，并使用vant的搜索组件，重新包装

#### 3.2 封装左侧菜单栏MenuList

#### 3.3 封装右侧内容区域CateContent

#### 3.4分类开发遇到的问题

- MenuList和CateContent组件在分类主模块中使用时，需要使用scroll组件包裹，并在scroll组件中传入滚动需要的条件

- 点击左侧菜单列表，右侧显示对应的内容模块

  - 在MenuList组件点击菜单列表事件中，使用$emit将每一个列表的index传到分类主模块中

    ```js
    changeColor(index){
          this.currentIndex= index  //在data数据中定义的当前索引与菜单栏每一列的索引相等时，改变当前项的样式
          this.$emit('menuClick',index)
    }
    ```

  - 在主模块绑定的菜单点击事件中，将index传入CateContent组件的数据，作为索引值,改变内容区相应的数据

    ```js
    menuClick(index){
          this.rightContent = this.messages[index].children
          this.$refs.scroll.scrollTo(0,0,0) //点击菜单栏每一项列表时，让右侧内容区域都重新滚回顶部
        }
    ```

- 分类主模块的数据使用本地存储

  ```js
  // 1.把获取的接口数据存储到本地
  const parsedCates = JSON.stringify(this.messages)
  ocalStorage.setItem('cates',parsedCates)
  //以上时获取数据时的操作
  --------------------------------------------------------------------------
  created(){
      // 2.获取本地存储的数据
      const Cates = JSON.parse(localStorage.getItem('cates'))
      if(!Cates){
        // 没有本地的数据，则发送请求获取数据
        this.getCateData()
      }else{
        // 有本地数据
        this.messages = Cates
        this.rightContent = this.messages[0].children
      }
  
      // this.getCateData()
    }, 
  ```


### 商品列表开发

#### 4.1使用封装过的navbar组件，作为导航栏

#### 4.2利用动态路由传值,获取页面数据

根据接口请求参数，从商品分类页面中的右侧内容区域，得到当前点击商品的id获取数据

```js
//router(index.js)
{
    path:'/shoplist/:cat_id',
    name:'ShopList',
    component: ShopList
  },
```

```html
//商品分类页面中的右侧内容区域
<router-link :to="{path:'/shoplist/'+ goods.cat_id}">
</router-link>
```

```js
//商品列表获得传过来的id
this.queryparams.cat_id = this.$route.params.cat_id
```

#### 4.3使用封装的scroll组件，包裹内容区域

#### 4.4使用BackTop组件

#### 4.5列表开发遇到的问题

1.获取到的内容都是一样的（可能请求的api数据有问题，没有请求参数也可以得到数据，改变了商品id获取的数据也是一样的，哎，弄了好久···)

2.加载下一页

- 计算获取数据的总页数

  总的商品条数[获取的数据有显示] / 每页的数量[在请求参数中设定的pagesize:10]

- 让当前页数与总页数比较大小，小于就加载下一页

3.获取的商品没有图片，需要判断，但是需要引用线上的图片地址，引入本地的图片不起作用

```html
<img :src="good.goods_small_logo?good.goods_small_logo:'https://img-blog.csdnimg.cn/20200628145700763.jpg'" alt="">
```

### 商品详情开发

#### 5.1 根据Vant封装ShopSwiper

#### 5.2利用动态路由传值,获取页面数据

根据商品列表页面每个商品传递的id,获取当前商品详细信息

#### 5.3详情开发遇到的问题

1.从详情页返回列表页面，列表页面不停留在一开始离开页面时的位置，始终会滚动到顶部（一直没有解决）？

2.加入购物车事件

```js
addShopCart(){
    
      // let goods_image
      // if(this.banners[0].pics_sma){
      //   goods_image = this.banners[0].pics_sma
      // }else{
      //   goods_image = "https://img-blog.csdnimg.cn/20200628145700763.jpg"
      // }
      // 1.获取购物车需要展示的信息
      const product = {}
      // 没有图片的会报错，自己想的方法解决不了
      product.image = this.banners[0].pics_sma 
      product.title = this.message.goods_name
      product.price = this.message.goods_price
      product.goods_id = this.goods_id

      // 2.将商品添加到购物车(promise、mapActins)
      // this.$store.dispatch('addCart',product).then(res =>{
      //   console.log(res)
      // })
      this.addCart(product).then(res =>{ 
        this.$toast.show(res,2000)
      })
      
    }
```

### 购物车页面开发

#### 6.1封装CartList组件（购物车里的每一项商品）

使用scroll组件包裹，购物车的数据使用vuex管理

#### 6.2封装CartItem组件（每一项商品的详细信息）

#### 6.3封装CartTool组件（底部工具栏）

- 全选按钮

  显示状态：判断商品是否有没有被选中的，如果有就显示不选中，反之显示选中；

  点击事件：

  - 如果原来都是选中的，点击一次，全部不选中
  - 如果原来都是没选中（某些没选中），点击，全部选中

- 计算总价格

- 去计算

#### 6.4使用vuex管理购物车的数据

1.下载vuex插件

`npm  i vuex -S`

2.创建store对象,保存在购物车组件中共享的状态：cartList

```js
//store目录下新建的index.js文件
import Vue from 'vue'
import Vuex from 'vuex'
import mutations from './mutations'
import actions from './action'
import getters from './getters'
//  1.安装插件
Vue.use(Vuex)

const state = {
  cartList:[] //存储购物车里面的商品
}

// 2.创建Store对象
const store = new Vuex.Store({
  state,
  mutations,
  actions,
  getters
})

// 3.导出store,挂载到Vue实例上
export default store
```

3.新建mutation-types.js, 并且在其中定义我们需要的常量.

```js
export const ADD_COUNTER = 'add_counter'
export const ADD_TO_CART = 'add_to_cart'
```

4.新建mutation.js,修改state中的状态

```js
// mutations唯一的目的就是修改state中的状态
// mutations中的每个方法尽量完成的事件单一

import {
  ADD_COUNTER,
  ADD_TO_CART
} from './mutation-types'

export default {
  [ADD_COUNTER](state,payload){
    payload.count++
  },
  [ADD_TO_CART](state,payload){
    payload.isChecked = true
    state.cartList.push(payload)
  }
}
```

5.新建action.js,代替Mutation进行异步操作

```js

import {
  ADD_COUNTER,
  ADD_TO_CART
} from './mutation-types'


export default {
  // context 上下文
  addCart(context,payload){
    return new Promise((resolve) => {
      // payload 新添加的商品
      //1. 查找state定义的数组（cartlist)中是否有payload
      // 将购物车商品遍历,判断payload是否已经添加过购物车
      // let oldProduct = null
      // for(let item of state.cartList){
      //   if(item.goods_id === payload.goods_id){
      //     oldProduct = item
      //   }
      // }
      // 感觉oldProduct这个变量有点影响理解，相当于一个中间变量
      let oldProduct = context.state.cartList.find(item => item.goods_id === payload.goods_id)


      // 2.判断oldProduct
      if(oldProduct){
        // oldProduct.count += 1 //有值加一
        context.commit(ADD_COUNTER,oldProduct)
        resolve('当前商品数量加一')
      }else{
        payload.count = 1
        // state.cartList.push(payload)
        context.commit(ADD_TO_CART,payload)
        resolve('添加了新商品')
      }
    
    })
  }  
}
```

6.新建getters.js,从store中获取一些**state变异后的状态**

```js
export default{
  cartLength(state){
    return state.cartList.length
  },
  cartList(state){
    return state.cartList
  }
}
```

### 个人页面开发

静态页面

### 扩展

#### 1.单位转换

px转换为vw,依赖插件postcss-px-to-viewport

需要在src目录下新建postcss.config.js文件，配置单位转换规则

#### 2.提示框的实现（封装成插件）

1.先封装一个提示组件，实现具体提示效果

```vue
<template>
  <div class="toast" v-show="isShow">
    {{message}}
  </div>
</template>

<script>
export default {
  name:'Toast',
  data(){
    return{
      message:'',
      isShow:false
    }
  },
  methods:{
    show(message,duration=2000){
      this.isShow = true;
      this.message = message;

      setTimeout(() => {
        this.isShow = false;
        this.message = ''
      },duration)
    }
  }
}
</script>

<style>
.toast{
  font-size: 16px;
  position: fixed;
  top:50%;
  left:50%;
  padding: 10px 15px;
  transform:translate(-50%,-50%);
  color: #fff;
  background: rgba(10,10,10,.5);
  border-radius: 5px;
  z-index: 999;
}
</style>
```

2.创建一个index.js文件,引入toast组件，封装成插件

```js
import Toast from './Toast'
const obj = {
  
}
obj.install = function(Vue){
  // 1.创建组件构造器
  const toastContrustor = Vue.extend(Toast)
  // 2.根据组件构造器，用new的方式创建一个组件对象
  const toast = new toastContrustor()
  // 3.将组件对象，手动挂载到某个元素上
  toast.$mount(document.createElement('div'))

  // 4.toast.$el对应的就是div
  document.body.appendChild(toast.$el)

  Vue.prototype.$toast = toast
}
export default obj
```

3.在main.js文件中引入封装的toast插件，并安装

```js
import toast from 'components/common/toast'
// 安装toast插件
Vue.use(toast)
```

### 本项目中遇到的最大的坑

从商品列表页面进入商品详情页面然后返回，不管商品列表页面滑动到什么位置，始终会回到顶部，在网上查了好多资料都没解决（一直持续困扰我n多天，简直让人崩溃），我本来都不想管这个bug了的，可是面试需要用到，被人看到不太好，于是今天又来查找原因，终于找到了解决方法，终是老天不负我！

应该是由于执行顺序不正确，页面正在处于滚动状态的时候触发`refresh`函数才使得滚动失效。对滚动事件添加了一个setTimeout事件，让页面刷新后再实现滚动到离开页面时的目标位置

```js
 activated(){
      setTimeout(() => { //相当于宏任务
        this.$refs.scroll.scrollTo(0,this.stateY,0)
      }, 0)
      this.$nextTick(() => { //相当于微任务
        this.$refs.scroll.refresh() //不设置页面刷新，也无法实现具体效果，不是很理解
      });
     
  },
  deactivated(){
    this.stateY = this.$refs.scroll.getScrollY() 
  }
```

