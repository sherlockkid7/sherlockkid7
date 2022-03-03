---
title: 前端面试——Vue篇
date: 2020-07-14 10:08:14
tags: vue
categories: 面试
---

####  1.单页应用（SPA）的理解，及优缺点

通俗一点说就是指只有一个主页面的应用，浏览器一开始要加载所有必须的 html, js, css。利用路由机制（js）实现 HTML 内容的变换

优点：

1、用户体验好，内容的改变不需要重新加载整个页面，避免了不必要的跳转和重复渲染。基于这一点spa对服务器压力较小

2、前后端分离，前端进行交互逻辑，后端负责数据处理

3、页面效果会比较炫酷（比如切换页面内容时的专场动画）

缺点：

1、初次加载时耗时多（可用路由懒加载解决）

2、前进后退功能路由管理（由于是单页面不能用浏览器的前进后退功能，所以需要自己建立堆栈管理）

3、不利于seo(所有的内容都在一个页面中动态替换显示)

#### 2.MVVM

MVVM由View、ViewModel、Model三部分组成

- View（视图层）：可以简单的理解为DOM层。主要用于给用户展示各种信息
- ViewModel（视图模型层）：是Model和View之间的通信桥梁。
- Model（模型/数据层）：负责处理业务逻辑以及和服务器端进行交互

在MVVM的架构下，**View层和Model层并没有直接联系，而是通过ViewModel层进行交互。** 一方面它实现了数据绑定，将Model的改变实时的反应到View中；另一方面它实现了DOM监听，当DOM发生一些事件(点击、滚动、touch等)时可以监听到，并在需要的情况下改变对应的Data ，因此开发者只需关注业务逻辑，无需手动操作DOM

#### 3.Vue 是如何实现数据双向绑定的

Vue 数据双向绑定主要是指：数据变化更新视图，视图变化更新数据

采用**数据劫持结合发布者-订阅者模式**的方式，通过 `Object.defineProperty()` 来劫持各个属性的 setter、getter，在数据变化时发布消息给订阅者，触发相应的监听回调。

Vue 主要通过以下 4 个步骤来实现数据双向绑定的：

1. 实现一个监听器 Observer，对数据对象进行递归遍历，包括子属性对象的属性，利用 Object.defineProperty() 对属性都加上 setter 和 getter
   这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化
2. compile 解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图
3. Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁，主要的任务是订阅 Observer 中的属性值变化的消息，当收到属性值变化的消息时，触发解析器 Compile 中对应的更新函数。
4. MVVM 作为数据绑定的入口，整合 Observer、Compile 和 Watcher 三者，**通过Observer来监听自己的 model 数据变化，通过 Compile 来解析编译模板指令，最终利用 Watcher 搭起 Observer 和 Compile 之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据 model 变更的双向绑定效果。**

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020071415034244.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)

响应式系统简述:

- 任何一个 Vue Component 都有一个与之对应的 Watcher 实例

- Vue 的 data 上的属性会被添加 getter 和 setter 属性

- 当 Vue Component render 函数被执行的时候, data 上会被 触碰(touch), 即被读, getter 方法会被调用, 此时 Vue 会去记录此 Vue component 所依赖的所有 data。(这一过程被称为依赖收集)

- data 被改动时（主要是用户操作）, 即被写, setter 方法会被调用, 此时 Vue 会去通知所有依赖于此 data 的组件去调用他们的 render 函数进行更新

  

#### 4.v-show 与 v-if 有什么区别

v-if和v-show都是用来**控制元素/组件的渲染**。**v-if** 是真正的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建，如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。**v-show** 不管初始条件是什么，元素总是会被渲染，通过设置DOM元素的display样式属性控制显隐

适用场景：如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，不需要频繁切换条件，则使用 v-if 较好。

#### 5.watch、methods 和 computed 的区别

- **computed** 

  **计算属性**，依赖其它属性值。自动**监听依赖值**的变化，从而**动态返回内容**。computed的值是会被缓存的。如果所依赖的数据发生改变时候, 就会重新计算。

  **computed 应用场景**

  当我们需要进行复杂逻辑的数据计算，需要的数据依赖于其他的数据的话，应该使用 computed。因为可以利用 computed 的缓存特性，避免每次获取值时，都要重新计算

- **methods**

  是一个方法，它可以接受参数，用于**存储监听事件回调函数**。每次调用, 都会重新执行函数。

  methods没有缓存，所以每次访问都要重新执行。当不需要缓存功能，就使用methods

- **watch** 是**侦听属性**，为了监听某个**响应数据**的变化。

  **watch的使用场景**

  需要在数据变化时执行异步或开销较大的操作时（一个数据影响多个数据），才用 watch。

扩展：

计算属性的setter和getter,计算属性默认只有 getter

**watch 和 computed的区别是：**

相同点：他们两者都是观察页面数据变化的。

不同点：computed只有当依赖的数据变化时才会计算, 当数据没有变化时, 它会读取缓存数据。
watch每次都需要执行函数。watch更适用于数据变化时的异步操作。

#### 6.Vue单向数据流（有点难以理解）

Vue 中，父子组件之间，prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来。这是为了防止子组件无意修改了父组件的状态——这会让应用的数据流难以理解

另外，每次父组件更新时，子组件的所有 prop 都会更新为最新值。这意味着你不应该在子组件内部改变 prop 。如果你这么做了，Vue 会在控制台给出警告

通常有两种改变 prop 的情况：

1. prop 作为初始值传入，子组件之后只是将它的初始值作为本地数据的初始值使用；
2. prop 作为需要被转变的原始值传入。（定义一个 computed 属性，此属性从 prop 的值计算得出。）

注意在 JavaScript 中对象和数组是引用类型，指向同一个内存空间。所以对于一个数组或对象类型的 prop 来说，在子组件中改变这个对象或数组本身将会影响到父组件的状态。

#### 7.Vue 生命周期

 Vue 实例从创建到销毁的过程，主要包括四个阶段：创建、挂载、更新、销毁

**beforeCreate（创建前）**

**created（创建后）**

**beforeMount（挂载前）**

**mounted（挂载后）**

**beforeUpdate（更新前）**

**updated（更新后）**

**beforeDestroy（销毁前）**

**destroyed（销毁后）**

**activited**：keep-alive 专属，组件被激活时调用
**deactivated**：keep-alive 专属，组件被销毁时调用

平时比较常用的是created和mounted。

created 钩子函数中调用异步请求,获取后台数据

mounted钩子函数中通过插件操作页面的DOM节点

#### 8.keep-alive 组件的理解

keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，避免重新渲染 ，其有以下特性：

- 一般结合路由和动态组件一起使用，用于缓存组件；
- 提供 include 和 exclude 属性，两者都支持字符串或正则表达式， include 表示只有名称匹配的组件会被缓存，exclude 表示任何名称匹配的组件都不会被缓存 ，其中 exclude 的优先级比 include 高；
- 对应两个钩子函数 activated 和 deactivated ，当组件被激活时，触发钩子函数 activated，当组件被移除时，触发钩子函数 deactivated。

适用场景：

有两个页面，商品列表和商品详情页面，一般我们会经常执行打开详情=>返回列表=>打开详情这样的操作，那么就可以对列表组件使用keep-alive组件进行缓存，这样每次返回列表的时候，都是从缓存中快速渲染，而不是重新渲染

#### 9.组件中 data 为什么必须是一个函数？

Vue组件可能存在多个实例，如果使用对象形式定义data，则会导致它们共用一个data对象，那么状态变更将会影响所有组件实例；而采用函数形式定义，那么每个组件实例会返回一个全新的data对象，组件实例之间的 data 属性值不会互相影响。而在Vue根实例创建过程中则不存在该限制，因为根实例只能有一个，不需要担心这种情况

#### 10.v-model 的原理

实现表单元素和数据的双向绑定
v-model其实是一个语法糖，它本质上是包含两个操作：
1.v-bind绑定一个value属性
2.v-on指令给当前元素绑定input事件

```js
//子组件
<template>
    <div>
        <input
          v-on:input="$emit('input', $event.target.value)"
          v-bind:value="value">
    </div>
</template>
<script>
export default {
  name:'child',
  props:{
    value:String
  }
}
</script>
-------------------------------------------------------------------
//父组件
<child :value="name"></child> 
<input type="text" :value="name" @input="name=$event.target.value" >
<p>{{name}}</p>
<button @click="change">按钮</button>
<script>
import child from './child.vue'
export default {
data(){
 return{
   name:"你好",
 }
},
components:{
   child,
},
methods:{
  change:function(){
     this.name = "世界";
}
},
</script>

```



#### 11.Vue 组件间通信

- ##### 父子组件间通信

  - 父传子**（通过props）**

    父组件通过import引入子组件，并注册，在子组件标签上添加需要传递的属性。在子组件中通过props接收。props的值有两种方式：

    方式一：字符串数组，数组中的字符串就是传递时的名称。

    方式二：对象，对象可以设置传递的类型，也可以设置默认值等

  - 子传父（通过$emit）

    子组件通过绑定事件触发函数，在函数中设置this.$emit('要派发的自定义事件'，传递的值)，在父组件中，通过v-on监听子组件派发的自定义事件

- ##### 兄弟组件间通信

  方法一：通过事件总线（event bus）实现

  在vue的原型对象上初始化bus,作为全局的事件总线（即两个组件之间的桥梁）。在一个组件中通过this.bus.$emit('自定义事件'，值)发送数据，在另一个组件中通过this.bus.$on('自定义事件'，function(){})接收数据

  方法二：通过Vuex实现

#### 12.Vuex的理解

Vuex是一个状态管理工具，每个Vuex应用的核心就是store对象（仓库），包含着应用中所有需要共享的组件状态。主要用于解决大中型复杂项目数据共享问题。

五大核心属性：

- State（单一状态树）

  存储数据、状态

- Getter

  可以认为是 store 的计算属性，从其中获取一些**state变异后的状态**

- Mutation

  是唯一更改 store 中状态的方法，且必须是同步函数（方便devtools跟踪每一个状态的变化）

- Action

  可以包含任意异步操作，通过提交mutation间接更新状态

- Module

  将 store 分割成模块，每个模块都具有state、mutation、action、getter、甚至是嵌套子模块。

vuex的数据传递流程：

当组件进行数据修改的时候，通过调用dispatch来触发actions，actions通过commit来触发mutations里面的方法进行数据的修改。mutations里面的每个函数都会有一个state参数，这样就可以在mutations里面进行state的数据修改，从而同步到组件，更新其数据状态

#### 13.vue-router 路由模式

- ##### **history 模式**

  利用 HTML5 History API 来实现 URL 的变化。

  其中做最主要的 API 有以下两个：history.pushState() 和 history.repalceState()。这两个 API 可以在不进行刷新的情况下，操作浏览器的历史纪录。唯一不同的是，前者是新增一个历史记录，后者是直接替换当前的历史记录

- ##### **hash 模式**

  使用 URL hash 值来作路由。hash 值的改变，都会在浏览器的访问历史中增加一个记录。因此我们能通过浏览器的回退、前进按钮控制hash 的切换。可以使用 hashchange 事件来监听 hash 值的变化，从而对页面进行跳转（渲染）

#### 14.虚拟 DOM

 虚拟DOM是为了解决浏览器性能问题而设计出来的

- 虚拟DOM本质上是JavaScript对象,是对真实DOM的抽象
- 状态变更时，记录新树和旧树的差异
- 最后把差异更新到真正的dom中

举例说明：一次操作中有10次更新dom的动作，虚拟dom不会立即操作dom，而是使用而是将这 10 次更新的 `diff` 内容保存到本地一个 JS 对象中，最终将这个 JS 对象一次性 `attch` 到 DOM 树上

优点：

- 无需手动操作dom
- 跨平台

#### 15.Vue 的 nextTick 的原理是什么？

主要作用：为了处理数据动态变化后，dom未及时更新

 nextTick 的原理正是 vue 通过异步队列控制 dom更新和 nextTick 回调函数先后执行的方式

#### 16.vue 首屏加载优化

1. vue 路由的懒加载

   `const  home = () => import('./Home.vue')`

2. 把不常改变的库放到 index.html 中，通过 cdn 引入,然后在vue.config.js文件中，使用webpack的externals属性将不需要打包的库文件分离出去，减少打包文件后的大小

   ```js
   externals: {
     'vue': 'Vue',
     'vue-router': 'VueRouter',
     'element-ui': 'ELEMENT',
   },
   ```

#### 17. `route` 和 `router` 的区别是什么？

`route`是“路由信息对象”，包括`path`,`params`,`hash`,`query`,`fullPath`,`matched`,`name`等路由信息参数。
`router`是“路由实例对象”，包括了路由的跳转方法(`push`、`replace`)，钩子函数等。

#### 18.Vue中key的作用

**key 的作用主要是为了高效的更新虚拟DOM**