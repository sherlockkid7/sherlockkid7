---
title: Vue模块化开发——学习笔记三
date: 2020-06-16 11:22:16
tags: vue
categories: Vue
---

## 基本介绍

常见的模块化规范：
CommonJS、AMD、CMD，也有ES6的Modules

前端标准的模块化规范：

- AMD - requirejs
- CMD - seajs

服务器端的模块化规范：CommonJS - Node.js

模块：一个js文件就是一个模块，模块内部的成员都是相互独立

## CommonJS

模块化有两个核心：导出和导入
CommonJS的导出：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061615354771.png#pic_center)

CommonJS的导入：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200616153606986.png#pic_center)

## ES6模块化

### export基本使用

- export指令用于导出变量

  ```js
  export let name = 'es6'
  export let age = 100
  
  //另一种写法
  let name = 'es6'
  let age = 100
  export {name,age}
  ```

- 导出函数或类

  ```js
  function test(content){
      ...
  }
  
  class Person{
      constructor(name,age){
          this.name = name;
          this.age = age;
      }
  }
  export{test,Person}
  ```

  

- export default

  某些情况下，一个模块中包含某个的功能，我们并不希望给这个功能命名，而且让导入者可以自己来命名,这个时候就可以使用export default

  需要注意：export default在同一个模块中，不允许同时存在多个。

### import使用

export指令导出了模块对外提供的接口，我们就可以通过import命令来加载对应的这个模块了

import指令用于导入模块中的内容，比如main.js的代码

```js
import {name,age,height} from "./main.js"
```

如果我们希望某个模块中所有的信息都导入，通过`*`可以导入模块中所有的export变量,但是通常情况下我们需要给`*`起一个别名，方便后续的使用

```js
import * as info from "./main.js"
```

