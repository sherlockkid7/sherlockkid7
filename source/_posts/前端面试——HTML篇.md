---
title: 前端面试——HTML篇
date: 2020-07-06 15:17:41
tags: html
categories: 面试
---

### HTML5的新特性

- 标签

  新增语义化标签（` header / footer / nav/ aside / section /`等）

  增加多媒体标签`video`和`audio`

  语义化标签有具体的含义，使得样式和结构更加分离

- 属性

  增强表单，主要是增强了`input`的type属性；`meta`增加charset以设置字符集；`script`增加async以异步加载脚本

- 存储

  增加`localStorage`、`sessionStorage`和`indexedDB`，引入了`application cache`对web和应用进行缓存

- API

  增加`拖放API`、`地理定位`、`SVG绘图`、`canvas绘图`、`Web Worker`、`WebSocket`

### doctype的作用是什么？

声明文档类型，告知浏览器用什么文档标准解析

- 怪异模式：浏览器使用自己的模式解析文档，不加doctype时默认为怪异模式
- 标准模式：浏览器以W3C的标准解析文档

### cookie,localStorage和sessionStorage

- **cookies**

  一般由服务器生成，可以设置失效时间，大小只有4K左右，HTTP请求头会自动带上cookie，需要存取时封装

- **localStorage**

  持久性存储，即使页面关闭也不会被清除，以名值对的方式存储，大小为5M

  localStorage 注意点：

  1. localStorage 只能存字符串，存取 JSON 数据需配合 JSON.stringify() 和 JSON.parse()
  2. 遇上禁用 setItem 的浏览器，需要使用 try...catch 捕获异常

- **sessionStorage**

  仅在当前会话下有效，关闭页面或浏览器后被清除，操作及大小同localStorage

localStorage和sessionStorage有自己存取的方法,`存储setItem(),获取getItem(),删除removeItem(),清空clear()` ，如：localStorage.setItem(‘属性’，值) 

### 浏览器渲染的步骤

1. HTML 解析，生成 DOM Tree
2. CSS 解析，生成 Style Rules
3. 两者关联生成 Render Tree（渲染树）
4. 根据渲染树进行Layout（布局）
5. 根据计算好的信息进行绘制，渲染整个页面

> 浏览器解析文档的过程中，如果遇到 script 标签，会立即解析脚本，停止解析文档（因为 JS 可能会改变 DOM 和 CSS,如果继续解析会造成浪费）。
> 如果是外部 script, 会等待脚本下载完成之后在继续解析文档。现在 script 标签增加了 defer 和 async 属性，脚本解析会将脚本中改变 DOM 和CSS 的地方解析出来，追加到 DOM Tree 和 Style Rules 上

##### 如何根据浏览器渲染机制加快首屏速度

1.**优化文件大小**：HTML和CSS的加载和解析都会阻塞渲染树的生成，从而影响首屏展示速度，因此我们可以通过优化文件大小、减少CSS文件层级的方法来加快首屏速度

2.**避免资源下载阻塞文档解析**：浏览器解析到`<script>`标签时，会阻塞文档解析，直到脚本执行完成，因此我们通常把`<script>`标签放在底部，或者加上`defer、async`

### 回流（重排）和重绘的理解

- 回流

  当元素的尺寸或者位置发生了变化，浏览器需要重新计算元素的几何属性，然后再将计算的结果绘制出来。这个过程就是回流（也叫重排）

  触发回流的情况：

  1.DOM元素的几何属性(`width/height/padding/margin/border`)发生变化

  2.DOM元素移动或增加

  3.计算 `offset/scroll/client`等属性

  4.调用`window.getComputedStyle`

- 重绘

  DOM样式发生了变化，但没有影响DOM的几何属性（比如修改了颜色或背景色）时

  浏览器不需重新计算元素的几何属性、直接为该元素绘制新的样式会触发重绘

### 常见HTTP状态码有哪些

> 2xx 开头（请求成功）

`200 OK`：客户端发送给服务器的请求被正常处理并返回

> 3xx 开头（重定向）

`301 Moved Permanently`：永久重定向，请求的网页已永久移动到新位置。服务器返回此响应时，会自动将请求者转到新位置

`302 Moved Permanently`：临时重定向，请求的网页已临时移动到新位置。服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求

`304 Not Modified`：未修改，自从上次请求后，请求的网页未修改过。服务器返回此响应时，不会返回网页内容

> 4xx 开头（客户端错误）

`400 Bad Request`：错误请求，服务器不理解请求的语法，常见于客户端传参错误

`401 Unauthorized`：未授权，表示发送的请求需要有通过 HTTP 认证的认证信息，常见于客户端未登录

`403 Forbidden`：禁止，服务器拒绝请求，常见于客户端权限不足

`404 Not Found`：未找到，服务器找不到对应资源

> 5xx 开头（服务端错误）

`500 Inter Server Error`：服务器内部错误，服务器遇到错误，无法完成请求

`501 Not Implemented`：尚未实施，服务器不具备完成请求的功能

`502 Bad Gateway`：作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应。

`503 service unavailable`：服务不可用，服务器目前无法使用（处于超载或停机维护状态）。通常是暂时状态。

### 常见的兼容性问题

pc端：

- 一般是css兼容的问题，例如：利用css hack来解决 , 如* , _ ,等

- JS有特性匹配来解决某个JS方法是否存在

- input设置type= number,pc端出现上下箭头

  ```
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button{
  	-webkit-appearance: none !important;
  	margin:0;
  }
  ```

手机端：

- 主要遇到过移动端300ms延迟问题，可以通过fastclick插件解决

- ios下取消input在输入的时候,英文首字母的默认大写

  ```html
  <input autocapitalize="off" autocorrect="off" />
  ```

- 页面有一个input输入框，需要输入手机号码，手机端输入时会弹出数字键盘，需要设置`type属性为tel`

- 拍照功能   添加图片 => 拍照功能

  ```html
  <a href="javascript:;" class="u-choose-btn" data-use="shuidian" data-useid="2" onclick="document.getElementById('myfile').click()">添加图片</a>
    
  <input type="file" accept="image/*" capture="camera" id="myfile" style="display:none">
    
  files[]
  ```

- **H5开发的时候在IOS设备上的一些兼容性问题的解决方法**
  1.设置overflow-y时滚动区域不流畅问题

  ```css
  -webkit-overflow-scrolling: touch;
  ```

#### IE8如何支持语义化标签

直接引入一个库 html5shiv.js,原理就是把语义化标签在低版本浏览器转化成了块级元素,让浏览器可以解析

#### 谷歌浏览器如何显示12px以下的字号

中文版的chrome有个12px字体限制的问题，就是当字体小于12px时候,都以12px来显示。这个问题在中文网站中并不突出，因为中文字体为了显示清晰一般都定义为大于或等于12px，但如果是一些英文网站那就不好说了，这时12px的限制就会破坏页面的美感，甚至因为文字变大而导致页面变形。

我们可以通过**css3的缩放**来实现这个问题,比方说我要展示10px的文字,可以通过设置字体20px,然后scale(0.5)。

### 性能优化相关问题

- #### **减少http请求**

  合并文件

  不使用CSS @import

  **图片处理**

  - 雪碧图
  - Base64
  - 使用字体图标来代替图片
  - 图片懒加载

- #### **减少重绘回流**

  DOM优化

  - 缓存DOM
  - 减少DOM深度及DOM数量
  - 事件代理
  - 防抖和节流

- #### **webpack优化**

  扩展：webpack的理解

  webpack是一个前端模块化打包构建工具，webpack本身需要的入口文件通过entry来指定，出口通过output来指定，默认只支持js文件，其它文件类型需要对应的loader来转换。例如，css => style-loader,css-loader

  当然本身还有一些内置插件来对文件进行压缩、合并等操作

详细资料：[前端性能优化的七大手段](https://www.cnblogs.com/xiaohuochai/p/9178390.html)

参考资料：[2万字！90个前端开发面试必问基础大总结](https://mp.weixin.qq.com/s/anrtDgzYhEAmaQ3kXzcqkw)