---
title: WebAPI
date: 2020-06-05 11:50:52
tags: JS
categories: JS
---



![](https://user-gold-cdn.xitu.io/2020/6/5/17282a52b579827e?w=3111&h=10775&f=png&s=2950128)

## 1.基本介绍

API（Application Programming Interface,应用程序编程接口）是一些预先定义的函数，帮助我们实现某种功能

Web API 是浏览器提供的一套操作浏览器功能和页面元素的 API ( BOM 和 DOM )。

## 2.DOM

文档对象模型（Document Object Model，简称DOM），是 W3C组织推荐的处理可扩展标记语言（html或者xhtml）的标准编程接口。
**文档**：一个页面就是一个文档，DOM中使用**document**表示
**节点**：网页中的所有内容，在文档树中都是节点（标签、属性、文本、注释等），使用**node**表示
**标签节点**：网页中的所有标签，通常称为**元素节点**，又简称为“元素”，使用**element**表示

###  2.1获取元素

- ID 获取

  - 语法：document.getElementById('id')
  - 作用：根据ID获取元素对象
  - 参数：id值，区分大小写的**字符串**
  - 返回值：元素对象 或 null

- 标签名获取

  - 语法： 

    1.**document**.getElementsByTagName('标签名')   

    2.**element**.getElementsByTagName('标签名') 

  - 作用：根据标签名获取元素对象

  - 参数：标签名

  - 返回值：元素对象集合（伪数组，数组元素是元素对象）

    注意：

    1.因为得到的是一个对象的集合，所以我们想要操作里面的元素就需要遍历。

    2.得到元素对象是动态的

    3.如果获取不到元素,则返回为空的伪数组

- HTML5 新增的方法获取

  ```js
  document.getElementsByClassName(‘类名’)；// 根据类名返回元素对象集合
  document.querySelector('选择器'); // 根据指定选择器返回第一个元素对象
  document.querySelectorAll('选择器'); // 根据指定选择器返回所有元素对象集合
  ```

  注意：querySelector 和querySelectorAll里面的选择器需要加符号

  `document.querySelector('#nav');`

- 特殊元素获取
  - 获取body元素
    `doucumnet.body // 返回body元素对象`
  - 获取html元素
     `document.documentElement // 返回html元素对象`

### 2.2事件基础

- 事件三要素

  - 事件源（谁）：触发事件的元素

  - 事件类型（什么事件）： 例如 click 点击事件
  - 事件处理程序（做啥）：事件触发后要执行的代码(函数形式)，事件处理函数

- 执行事件的步骤

  - 获取事件源
  - 注册事件（绑定事件）
  - 添加事件处理程序（采取函数赋值形式）

- 常见的鼠标事件

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200604225603884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)

### 2.3操作元素

JavaScript的 DOM 操作可以改变网页内容、结构和样式，我们可以利用 DOM 操作元素来改变元素里面的内容、属性等。（注意：这些操作都是通过元素对象的属性实现的）

**获取属性的值**
元素对象.属性名
**设置属性的值**
元素对象.属性名 = 值
**表单元素**中有一些属性如：disabled、checked、selected，元素对象的这些**属性的值**是**布尔型**。

- 改变元素内容

  - element.innerText
    从起始位置到终止位置的内容, 但它去除 html 标签， 同时空格和换行也会去掉
  - **element.innerHTML**(W3C标准)
    起始位置到终止位置的全部内容，包括 html 标签，同时保留空格和换行

- 常用元素的属性操作

  1. nnerText、innerHTML 改变元素内容

  2. src、href
  3. id、alt、title

- 表单元素的属性操作

  type、value、checked、selected、disabled

- 样式属性操作 

  - element.style 行内样式操作
    `元素对象.style.样式属性 = 值;`
    **注意**：
    1.JS 里面的样式采取驼峰命名法 比如 fontSize、 backgroundColor
    2.JS 修改 style 样式操作，产生的是行内样式，CSS 权重比较高

  - element.className 类名样式操作
    `元素对象.className = 值;`
    **注意**：

    1.如果样式修改较多，可以采取操作类名方式更改元素样式。

    2.class因为是个保留字，因此使用className来操作元素类名属性

    3.className 会直接更改元素的类名，会覆盖原先的类名。

-  自定义属性的操作
   **H5自定义属性**
   自定义属性目的：是为了**保存并使用数据**。有些数据可以保存到页面中而不用保存到数据库中
   **1. 设置H5自定义属性**
   H5规定自定义属性data-开头做为属性名并且赋值。
   `<div data-index="1"></div>`
   或者使用 JS 设置
   `element.setAttribute(‘data-index’, 2)`
   **2. 获取H5自定义属性**
   兼容性获取 element.getAttribute(‘data-index’);
   H5新增 element.dataset.index 或者 element.dataset[‘index’]  ie 11才开始支持
   **获取属性值**
   element.属性    获取内置属性值（元素本身自带的属性）
   `element.getAttribute(‘属性’);` 主要获得自定义的属性 （标准） 
   **设置属性值**
   element.属性    设置内置属性值
   `element.setAttribute(‘属性’);` 主要设置自定义的属性 （标准）
   **移除属性**
   `element.removeAttribute('属性');`

### 2.4节点操作

- 节点概述

  一般地，节点至少拥有nodeType（节点类型）、nodeName（节点名称）和nodeValue（节点值）这三个基本属性。 

  - 元素节点 nodeType 为 1 
  - 属性节点 nodeType 为 2 
  - 文本节点 nodeType 为 3 （文本节点包含文字、空格、换行等）
    我们在实际开发中，节点操作主要操作的是**元素节点**

- 节点层级

  利用 DOM 树可以把节点划分为不同的层级关系，常见的是父子兄层级关系。

  1.父级节点
  `node.parentNode` 

  - parentNode 属性可返回某节点的父节点，注意是最近的一个父节点
  - 如果指定的节点没有父节点则返回 null

  2.子节点

  - 所有子节点  
    parentNode.childNodes（标准）
    parentNode.childNodes 返回包含指定节点的子节点的集合，该集合为即时更新的集合。
    注意：返回值里面包含了所有的子节点，包括元素节点，文本节点等。
    如果只想要获得里面的元素节点，则需要专门处理。 所以我们一般不提倡使用childNodes

  - **子元素节点**
    parentNode.**children**（非标准）
    parentNode.children 是一个只读属性，返回所有的子元素节点。它只返回子元素节点，其余节点不返回 （**这个是我们重点掌握的**）。

    parentNode.**firstChild**  
    firstChild 返回第一个子节点，找不到则返回null。同样，也是包含所有的节点。parentNode.**lastChild**  最后一个子节点
    lastChild 返回最后一个子节点，找不到则返回null。同样，也是包含所有的节点。

    parentNode.**firstElementChild** 
    firstElementChild 返回第一个子元素节点，找不到则返回null。parentNode.**lastElementChild** 
    lastElementChild 返回最后一个子元素节点，找不到则返回null。
    注意：这两个方法有兼容性问题，IE9 以上才支持。

    实际开发中，firstChild 和 lastChild 包含其他节点，操作不方便，而 firstElementChild 和lastElementChild 又有兼容性问题，那么我们如何**获取第一个子元素节点**或**最后一个子元素节点**呢？

    **解决方案：**

    1. 如果想要第一个子元素节点，可以使用 **parentNode.chilren[0]** 
    2. 如果想要最后一个子元素节点，可以使用**parentNode.chilren[parentNode.chilren.length - 1]** 

  3.兄弟节点

  ​		node.**nextSibling**
  ​		nextSibling 返回当前元素的下一个兄弟节点，找不到则返回null。同样，也是包含所有的节点。	
  ​		node.**previousSibling** 
  ​		previousSibling 返回当前元素上一个兄弟节点，找不到则返回null。同样，也是包含所有的节点。

  ​		注意：这下面两个方法有兼容性问题， IE9 以上才支持。
  ​		node.nextElementSibling
  ​		nextElementSibling 返回当前元素下一个兄弟元素节点，找不到则返回null。	
  ​		node.previousElementSibling 
  ​		previousElementSibling 返回当前元素上一个兄弟节点，找不到则返回null。	

- 创建节点

  **document.createElement('tagName')**
  document.createElement() 方法创建由 tagName 指定的 HTML 元素。因为这些元素原先不存在，是根据我们的需求动态生成的，所以我们也称为**动态创建元素节点**。

- 添加节点

  **node.appendChild(child)** 
  node.appendChild() 方法将**一个节点**添加到指定**父节点的子节点列表末尾**。类似于 CSS 里面的after 伪元素。

  **node.insertBefore(child, 指定元素)** 
  node.insertBefore() 方法将**一个节点**添加到**父节点的指定子节点前面**。类似于 CSS 里面的 before 伪元素。

- 删除节点

  node.removeChild() 方法从 DOM 中删除一个子节点，返回删除的节点。

- 复制节点

  node.cloneNode() 
  node.cloneNode() 方法返回调用该方法的节点的一个副本。
  注意：

  1.如果括号**参数为空**或者为 **false** ，则是**浅拷贝**，即只克隆复制节点本身，不克隆里面的子节点。

  2.如果括号**参数为 tru**e ，则是**深度拷贝**，会复制节点本身以及里面所有的子节点。

- 替换节点

  parentNode.replaceChild(newChild, oldChild); 
  用指定的节点替换当前节点的一个子节点，并返回被替换掉的节点。

- **三种动态创建元素区别**

  document.write() 
  element.innerHTML
  document.createElement() 
  区别：

  1. **document.write** 是直接将内容写入页面的内容流，会**导致页面**全部**重绘** 

  2. innerHTML 是将内容写入某个 DOM 节点，不会导致页面全部重绘 

  3. 如果页面**创建元素很多**，建议使用 **innerHTML** ，因其**效率更高**（不要拼接字符串，**采取数组形式拼接**） ，但是结构稍微复杂

  4. 如果页面创建元素较少，建议使用 createElement() ，结构更清晰
     总结：不同浏览器下，innerHTML 效率要比 creatElement 高

     **createElement方式**

     ```js
     <script>
         function fn() {
             var d1 = new Date();
             for (var i = 0; i < 1000; i++) {
                 var div = document.createElement('div');
                 div.style.width = '100px';
                 div.style.height = '2px';
                 div.style.border = '1px solid red';
                 document.body.appendChild(div);
             }
             var d2 = new Date();
             console.log(d2 - d1);
         }
         fn();
     </script>
     ```

     **innerHTML数组方式**

     ```js
     <script>
         function fn() {
             var d1 = new Date();
             var array = [];
             for (var i = 0; i < 1000; i++) {
                 array.push('<div style="width:100px; height:2px; border:1px solid blue;"></div>');
             }
             document.body.innerHTML = array.join('');
             var d2 = new Date();
             console.log(d2 - d1);
         }
         fn();
     </script>
     ```

     

## 3.BOM

BOM（Browser Object Model）即浏览器对象模型，它提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是 window。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605084652710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)

### 3.1window

window 对象是浏览器的顶级对象，它具有双重角色。 

1. 它是 JS 访问浏览器窗口的一个接口。 
2. 它是一个全局对象。定义在全局作用域中的变量、函数都会变成 window 对象的属性和方法。 
3. 在调用的时候可以省略 window， alert()、prompt()都属于 window 对象方法。 
   注意：window下的一个特殊属性 window.name

### 3.2window 对象的常见事件

- 窗口加载事件

  ```js
  window.onload = function(){};
  window.addEventListener("load",function(){});
  ```

  window.onload 是窗口 (页面）加载事件,当文档内容完全加载完成会触发该事件(包括图像、脚本文件、CSS 文件等)。
  注意：

  - 有了 window.onload 就可以把 JS 代码写到页面元素的上方，因为 onload 是等页面内容全部加载完毕， 再去执行处理函数。

  - window.onload 传统注册事件方式 只能写一次，如果有多个，会以最后一个为准。如果使用 addEventListener 则没有限制

  `document.addEventListener('DOMContentLoaded',function(){}) //Ie9以上才支持`
  DOMContentLoaded 事件触发时，仅当DOM加载完成，不包括样式表，图片，flash等等。
  如果页面的图片很多的话, 从用户访问到onload触发可能需要较长的时间, 交互效果就不能实现，必然影响用户的体验，此时用 DOMContentLoaded 事件比较合适。

- 调整窗口大小事件

  ```js
  window.onresize = function(){}
  window.addEventListener("resize",function(){});
  ```

  window.onresize 是调整窗口大小加载事件
  注意：

  - 只要窗口大小发生像素变化，就会触发这个事件。
  - 经常利用这个事件完成响应式布局。window.innerWidth 当前屏幕的宽度

###   3.3location 对象

​	    用于获取或设置窗体的 URL，并且可以用于解析 URL

- 统一资源定位符 (Uniform Resource Locator, URL) 

  是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的 URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它。

​		URL 的一般语法格式为：protocol://host[:port]/path/[?query]#fragment

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605093430290.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)

- location 对象的属性

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605093832429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70)

  重点记住： href 和 search

- location对象的常见方法

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605093832427.png)

###  3.4navigator 对象

navigator 对象包含有关浏览器的信息，它有很多属性，我们最常用的是 userAgent，该属性可以返回由客户端发送服务器的 user-agent 头部的值,可以判断用户使用什么终端打开页面，实现跳转

###  3.5history 对象

window 对象给我们提供了一个 history 对象，与浏览器历史记录进行交互。该对象包含用户（在浏览器窗口中）访问过的 URL。（ 一般在OA 办公系统中使用）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605093831302.png)

### 3.6定时器

- **setTimeout() 定时器**（炸弹）

  开启定时器：window.setTimeout(调用函数, [延迟的毫秒数]);
  setTimeout() 的调用函数我们也称为回调函数 callback，在**定时器到期后执行调用函数**
  注意：

  - window 可以省略。

  - 这个调用函数可以直接写函数，或者写函数名

  - 延迟的毫秒数省略默认是 0

  - 因为定时器可能有很多，所以我们经常给定时器赋值一个标识符。

  停止定时器：
  window.clearTimeout(timeoutID)
  clearTimeout()方法取消了先前通过调用 setTimeout() 建立的定时器。
  注意：里面的参数就是定时器的标识符 。

- **setInterval() 定时器（闹钟）**

  开启定时器：
  window.setInterval(回调函数, [间隔的毫秒数]);
  setInterval() 方法重复调用一个函数，**每隔这个时间，就去调用一次回调函数**。
  注意：

  - window 可以省略。

  - 这个调用函数可以直接写函数，或者写函数名。

  - 间隔的毫秒数省略默认是 0，如果写，必须是毫秒，表示每隔多少毫秒就自动调用这个函数。

  - 第一次执行也是间隔毫秒数之后执行，之后每隔毫秒数就执行一次。

  停止 setInterval() 定时器：
  window.clearInterval(intervalID);
  clearInterval()方法取消了先前通过调用 setInterval()建立的定时器。

###  3.7this指向问题

this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，一般情况下this的最终指向的是那个调用它的对象。

1. 全局作用域或者普通函数中this指向全局对象window（注意定时器里面的this指向window）
2. 方法调用中谁调用this指向谁
3. 构造函数中this指向构造函数的实例

## 4.事件高级

### 4.1注册事件

给元素添加事件，称为注册事件或者绑定事件。 
注册事件有两种方式：传统方式和方法监听注册方式

- 传统注册方式
  - 利用 on 开头的事件 onclick 
  - `<button onclick=“alert('hi~')”></button>`或者`btn.onclick = function() {}` 
    特点：同一个元素同一个事件只能设置一个处理函数，最后注册的处理函数将会覆盖前面注册的处理函数

- 方法监听注册方式(w3c 标准推荐方式)

  **addEventListener 事件监听方式** 

  `eventTarget.addEventListener(type, listener[, useCapture])` 

  eventTarget.addEventListener()方法将指定的监听器注册到 eventTarget（目标对象）上，当该对象触发指定的事件时，就会执行事件处理函数。 
  该方法接收三个参数： 

  - type：事件类型字符串，比如 click 、mouseover ，注意这里不要带 on 

  - listener：事件处理函数，事件发生时，会调用该监听函数 

  - useCapture：可选参数，是一个布尔值，默认是 false。

    特点：不支持IE9 之前的 IE，同一个元素同一个事件可以注册多个监听器,按注册顺序依次执行

  **attachEvent 事件监听方式**

  `eventTarget.attachEvent(eventNameWithOn, callback)` 

  eventTarget.attachEvent()方法将指定的监听器注册到 eventTarget（目标对象） 上，当该对象触

  发指定的事件时，指定的回调函数就会被执行。

  该方法接收两个参数：

  - eventNameWithOn：事件类型字符串，比如 onclick 、onmouseover ，这里要带 on

  - callback： 事件处理函数，当目标触发事件时回调函数被调用

    **注意：**IE8 及早期版本支持

### 4.2删除事件

- 传统方式
  eventTarget.onclick = null;
- 方法监听注册方式
  ① eventTarget.removeEventListener(type, listener[, useCapture]);
  ② eventTarget.detachEvent(eventNameWithOn, callback);

### 4.3DOM 事件流

事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程即 DOM 事件流。 

注意 :

- JS代码中只能执行捕获或者冒泡其中的一个阶段。 
- onclick 和 attachEvent 只能得到冒泡阶段。 
- addEventListener(type, listener[, **useCapture**])第三个参数如果是 **true**，表示在**事件捕** 
  **获阶段**调用事件处理程序；如果是 **false**（不写默认就是false），表示在**事件冒泡阶段**调用事件处理 
  程序。 
- 实际开发中我们很少使用事件捕获，我们更关注事件冒泡。 
- 有些事件是没有冒泡的，比如 onblur、onfocus、onmouseenter、onmouseleave

DOM 事件流会经历3个阶段：

1. 捕获阶段

   由 DOM 最顶层节点开始，然后逐级向下传播到到最具体的元素接收的过程。

2. 当前目标阶段

3. 冒泡阶段

   事件开始时由最具体的元素接收，然后逐级向上传播到到 DOM 最顶层节点的过程

   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605101429216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)

### 4.4事件对象

```js
eventTarget.onclick = function(event) {} 
eventTarget.addEventListener('click', function(event) {}） // 这个 event 就是事件对象
```

官方解释：event 对象代表事件的状态，比如键盘按键的状态、鼠标的位置、鼠标按钮的状态。 
简单理解：事件发生后，跟事件相关的一系列信息数据的集合都放到这个对象里面，这个对象就是事件对象event，它有很多属性和方法。

**事件对象本身的获取存在兼容问题：** 

1. 标准浏览器中是浏览器给方法传递的参数，只需要定义形参 e 就可以获取到。 

2. 在 IE6~8 中，浏览器不会给方法传递参数，如果需要的话，需要到 window.event 中获取查找。 
   解决: 
   e = e || window.event;

   只要“||”前面为false, 不管“||”后面是true 还是 false，都返回 “||” 后面的值。
   只要“||”前面为true, 不管“||”后面是true 还是 false，都返回 “||” 前面的值。

   ```js
   <script>
       var div = document.querySelector('div');
       div.onclick = function(e) {
               // 事件对象
               e = e || window.event;
               console.log(e);
       }
   </script>
   ```

**事件对象的属性和方法**

![在这里插入图片描述](https://img-blog.csdnimg.cn/202006051021537.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)

**e.target 和 this 的区别**：
**this** 是**事件绑定的元素**， 这个函数的调用者（绑定这个事件的元素）
**e.target** 是**事件触发的元素**。

通常情况下terget 和 this是一致的，但有一种情况不同，那就是在事件冒泡时（父子元素有相同事件，单击子元素，父元素的事件处理函数也会被触发执行），这时候this指向的是父元素，因为它是绑定事件的元素对象，而target指向的是子元素，因为他是触发事件的那个具体元素对象。

```js
//事件冒泡下的e.target和this
<ul>
    <li>abc</li>
    <li>abc</li>
    <li>abc</li>
</ul>
<script>
    var ul = document.querySelector('ul');
    ul.addEventListener('click', function(e) {
          // 我们给ul 绑定了事件  那么this 就指向ul  
          console.log(this); // ul
          // e.target 触发了事件的对象 我们点击的是li e.target 指向的就是li
          console.log(e.target); // li
    });
</script>
```

### 4.5阻止默认行为

> html中一些标签有默认行为，例如a标签被单击后，默认会进行页面跳转。

```js
<a href="http://www.baidu.com">百度</a>
<script>
    //阻止默认行为 让链接不跳转 
    var a = document.querySelector('a');
    a.addEventListener('click', function(e) {
         e.preventDefault(); //  dom 标准写法
    });
   //传统的注册方式
    a.onclick = function(e) {
        // 普通浏览器 e.preventDefault();  方法
        e.preventDefault();
        // 低版本浏览器 ie678  returnValue  属性
        e.returnValue = false;
        // 我们可以利用return false 也能阻止默认行为 没有兼容性问题
        return false;
    }
</script>
```

### 4.6阻止事件冒泡

- 标准写法：利用事件对象里面的 stopPropagation()方法
  e.stopPropagation()
- 非标准写法：IE 6-8 利用事件对象 cancelBubble 属性
  e.cancelBubble = true;

### 4.7事件委托

事件委托也称为事件代理,通俗的讲就是不给子元素注册事件，**给父元素注册事件**，把处理代码在父元素的事件中执行。

**事件委托的原理**
给父元素注册事件，利用事件冒泡，当子元素的事件触发，会冒泡到父元素，然后去控制相应的子元素。

**事件委托的作用**

- 只操作了一次 DOM ，提高了程序的性能。
- 动态新创建的子元素，也拥有事件。

### 4.8鼠标事件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605103724655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70)

1.禁止鼠标右键菜单

contextmenu主要控制应该何时显示上下文菜单，主要用于程序员取消默认的上下文菜单

```js
document.addEventListener('contextmenu', function(e) {
e.preventDefault();
})
```

2.禁止鼠标选中（selectstart 开始选中）

```js
document.addEventListener('selectstart', function(e) {
e.preventDefault();
})
```

**鼠标事件对象**

![img](https://img-blog.csdnimg.cn/20200605103724703.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70)

### 4.9键盘事件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605105020906.png#pic_center)

```js
    <script>
        // 常用的键盘事件
        //1. keyup 按键弹起的时候触发 
        document.addEventListener('keyup', function() {
            console.log('我弹起了');
        })

        //3. keypress 按键按下的时候触发  不能识别功能键 比如 ctrl shift 左右箭头啊
        document.addEventListener('keypress', function() {
                console.log('我按下了press');
        })
        //2. keydown 按键按下的时候触发  能识别功能键 比如 ctrl shift 左右箭头啊
        document.addEventListener('keydown', function() {
                console.log('我按下了down');
        })
        // 4. 三个事件的执行顺序  keydown -- keypress -- keyup
    </script>
```

**键盘事件对象**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605105350505.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605105351133.png)

**使用keyCode属性判断用户按下哪个键**

```js
    <script>
        // 键盘事件对象中的keyCode属性可以得到相应键的ASCII码值
        document.addEventListener('keyup', function(e) {
            console.log('up:' + e.keyCode);
            // 我们可以利用keycode返回的ASCII码值来判断用户按下了那个键
            if (e.keyCode === 65) {
                alert('您按下的a键');
            } else {
                alert('您没有按下a键')
            }
        })
        document.addEventListener('keypress', function(e) {
            // console.log(e);
            console.log('press:' + e.keyCode);
        })
    </script>
```

## 5.JS 执行机制
JavaScript 语言的一大特点就是单线程，也就是说，同一个时间只能做一件事。
### 同步
同步任务都在主线程上执行，形成一个执行栈。
### 异步
JS 的异步是通过回调函数实现的。
一般而言，异步任务有以下三种类型: 
1、普通事件，如 click、resize 等 
2、资源加载，如 load、error 等 
3、定时器，包括 setInterval、setTimeout 等
异步任务相关回调函数添加到任务队列中（任务队列也称为消息队列）。
### **JS执行步骤**
1. **先执行**执行栈中的**同步任务**。 
2. **异步任务**（回调函数）放入**任务队列**中。
3. 一旦执行栈中的所有**同步任务执行完毕**，系统就会按次序**读取**任务队列中的**异步任务**，于是被读取的异步任务结束等待状态，**进入执行栈**，开始**执行**。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200605111242706.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)
由于主线程不断的重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为事件循环（ event loop）。

