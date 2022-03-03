---
title: 前端面试——JavaScript篇
date: 2020-07-06 15:46:13
tags: JS
categories: 面试
---

#### 1.js 的数据类型有哪些,值（变量）是如何存储的

##### **基本数据类型**（原始数据类型）【七种】：

- Undefined

- Null

- Boolean

- Number

- String

- Symbol

  ES6新增的一种原始数据类型 ，表示为 **独一无二 的值**，用来定义独一无二的对象属性名。

  **Symbol的定义**

  - 一种Symbol类型可以通过使用Symbol()函数来生成；

  - Symbol()函数可以接收一个字符串作为参数

    ```js
    let s1 = Symbol('web');
    let s2 = Symbol('web');
    console.log(s1 === s2); // false ,Symbol()函数接收的参数相同，其变量的值不同，s1和s2是Symbol类型的变量，因为变量的值不同，所以打印的结果为false
    console.log(typeof s1); //symbol
    console.log(typeof s2); //symbol
    ```

- BigInt（es10新增,表示任意精度格式的整数）

基本数据类型：直接存放在**栈（stack）**中，在内存中以固定的大小存储

##### 引用数据类型【一种】：

- Object 

  Object本质上是由一组无序的名值对组成的。里面包含 function、Array、Date等

引用数据类型：在**栈（stack）**中存储指向其堆内存的地址（指针），值存放在**堆（heap）**中。当我们想要访问引用类型的值的时候，需要先从栈中获得对象的地址指针，然后通过地址指针找到其在堆中的数据。

##### **扩展：**（将一个变量赋值给另一个变量）

- 基本数据类型复制的是值，赋值完成，两个变量没有任何关系；

- 引用数据类型复制的是地址（指针），复制操作完成后，两个变量实际上将引用同一个对象，修改一个变量另一个变量也会跟着一起变化。

#### 2.对于基本数据类型和引用数据类型的理解

基本数据类型的值指的是简单的数据段，引用数据类型指的是可能有多个值构成的对象。可以从三个方面来理解：动态的属性、变量赋值、传递参数

- 动态的属性

  定义基本数据类型和引用数据类型的值，都是创建一个变量并为该变量赋值。两者的区别在于，引用数据类型的值，我们可以为其添加、改变和删除其属性和方法，而对于基本数据类型的值则不能

  ```js
  var person = new Object(); //创建一个对象并将其保存在变量person中
  person.name = "Song"; //为该对象添加一个名为name的属性，并赋值为Song
  console.log(person.name); //访问name这个属性 结果为Song
  ```

- 变量赋值

  一个变量向另一个变量复制（基本数据类型或引用数据类型的）值时，两则的区别：

  - 基本数据类型的值

    基本数据类型的值存储在栈中，当变量赋值时，会重新创建一个新值将其复制到为新变量分配的位置上,此时两个变量各自拥有属于自己的独立的内存空间，因此两者可以参与任何操作而不会相互影响。

    ```js
    var a = 1;
    var b = a;
    b = 2;
    console.log(a);//1
    console.log(b);//2
    ```

  - 引用数据类型的值

    引用数据类型的值存储在堆中，同时在栈中会有相应的堆地址（指针），指向堆的位置。当复制引用数据类型的值时，复制的不是堆内存中的值，而是将栈内存中的地址复制过去，复制操作结束后，两个对象实际上都指向堆中的同一个地方。因此改变其中一个对象（堆中的值改变），那么会影响到另一个对象。

    ```js
    var obj1 = {
      name:"Song"
    };
    var obj2 = obj1;
    obj2.name = "D"; //改变obj2的name属性的值，则将obj1的也改变了
    console.log(obj1.name);// D 
    ```

- 传递参数(函数的形参可以看做是一个变量)

  - **基本数据类型传参**
    当我们把一个基本类型变量作为参数传给函数的形参时，其实是把变量在栈空间里的值复制了一份给形参，那么在方法内部对形参做任何修改，都不会影响到的外部变量。

  - **引用数据类型传参**

    当我们把引用类型变量传给形参时，其实是把变量在栈空间里保存的堆地址复制给了形参，形参和实参其实保存的是同一个堆地址，所以操作的是同一个对象。

    ```js
    function test(person){
    person.age = 26;
    person = {  //重新开辟了一个内存空间，然后将此内存空间的地址赋给person，可以理解为将刚才指向p1的指针地址给覆盖了，所以改变了person的指向
    　　name:'yyy',
    　　age:30
    }
    return person
    }
    const p1 = {
    　　name:'yck',
    　　age:25
    };
    const p2 = test(p1);//将p1的内存地址指针复制给了形参person，两者引用的是同一个对象，这个时候在函数中改变变量，就会影响到外部。
    console.log(p1);//{name: "yck", age: 26}
    console.log(p2);//{name: "yyy", age: 30}
    ```

#### 3.JS中数据类型的判断

- ##### typeof

  typeof 检测**变量的数据类型**，返回一个**字符串**。

  对于基本类型，除了null都可以返回正确类型;对于对象来说，除了function都返回object。

  ```js
  console.log(typeof 2);               // number
  console.log(typeof true);            // boolean
  console.log(typeof 'str');           // string
  console.log(typeof undefined);       // undefined
  console.log(typeof null);            // object     null 的数据类型被 typeof 解释为 object
  -----------------------------------------------------------------
  console.log(typeof []);              // object     []数组的数据类型在 typeof 中被解释为 object
  console.log(typeof function(){});    // function
  console.log(typeof {});              // object
  ```

- ##### instanceof

  instanceof 用来判断**对象的类型**，原理是检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。也就是判断对象是否是某一数据类型（如Array）的实例

  ```
  object instanceof constructor
  //object  某个实例对象
  //constructor  某个构造函数
  ```

  ```js
  //使用instanceof来判断基本类型，则始终返回false
  console.log(2 instanceof Number);                    // false
  console.log(true instanceof Boolean);                // false 
  console.log('str' instanceof String);                // false  
  ----------------------------------------------------------------
  //引用数据类型的值都是object的实例，在检测一个引用类型值和Object构造函数时，instanceof操作符始终返回true。
  console.log([] instanceof Array);                    // true
  console.log(function(){} instanceof Function);       // true
  console.log({} instanceof Object);                   // true
  ```

- ##### **Object.prototype.toString()**

  Object.prototype.toString()方法检测对象类型

  使用 call 对基本数据类型进行包装，使用 Object 对象的原型方法 toString转成字符串 。传入基本类型也能够判断出结果

  ```js
  //结果都为true
  Object.prototype.toString.call({})  ===  '[object Object]'   
  Object.prototype.toString.call([])   ===  '[object Array]'　　
  Object.prototype.toString.call(() => {})  ===  '[object Function]'　　
  Object.prototype.toString.call('somestring')  ===  '[object String]'　　
  Object.prototype.toString.call(1)  ===  '[object Number]'　　
  Object.prototype.toString.call(true)  ===  '[object Boolean]'　　
  Object.prototype.toString.call(Symbol()) ===  '[object Symbol]'　　
  Object.prototype.toString.call(null)   ===  '[object Null]'　　
  Object.prototype.toString.call(undefined)  === '[object Undefined]'　
  ```

- ##### **constructor**

  ```js
  console.log((2).constructor === Number); // true
  console.log((true).constructor === Boolean); // true
  console.log(('str').constructor === String); // true
  console.log(([]).constructor === Array); // true
  console.log((function() {}).constructor === Function); // true
  console.log(({}).constructor === Object); // true
  
  ----------------------------------------------------------
  function Fn(){};
  Fn.prototype=new Array();
  var f=new Fn();
  console.log(f.constructor===Fn);    // false，函数Fn的原型改变了，constructor也会发生改变
  console.log(f.constructor===Array); // true 
  ```

#### 4.数据类型的转换

| 转换类型     | 方法                                                         |
| ------------ | ------------------------------------------------------------ |
| 转换为字符串 | **toString()**、String() 强制转化、**加号拼接字符串**        |
| 转换为数字型 | **parseInt()**、**parseFloat()**、Number()强制转换、js隐式转换（-  *  /） |
| 转换为布尔型 | Boolean()方法                                                |

**包装类型**

为了便于操作基本类型值，衍生出来了三个包装类型:Boolean,Number,String,每当读取一个基本类型值的时候，后台会创建一个对应的基本包装类型的对象，从而能够调用一些方法来操作这些基本类型。每个包装类型都映射到同名的基本类型。

**引用数据类型和包装类型的主要区别**：就是对象的生存期。使用new操作符创建的引用类型的实例，在执行流离开当前作用域之前都一直保存在内存中。而自动创建的基本包装类型的对象，则只存在于一行代码的执行瞬间，然后立即被销毁，因此我们不能在运行时为基本类型值添加属性和方法。

```js
var s1 = "stringtext";
s1.color = "red"; //在这一句话执行完的瞬间，第二行创建的String就已经被销毁了。
console.log(s1.color);//执行这一行代码时又创建了自己的String对象，而该对象没有color属性，结果为undefine
```

**宽松相等“==”和严格相等“===”有什么区别？**（“==”和“===”是隐式类型转换）

==在相等比较中会自动类型转换，而===不会自动类型转换，直接比较

扩展：[面试题](https://zhuanlan.zhihu.com/p/31105614)

#### 5.作用域和作用域链

- ##### 作用域

  作用域是定义变量（函数）时产生的，决定了代码执行区域对于变量，函数，对象的可访问性

  **作用域最大的用处就是隔离变量，不同作用域下同名变量不会有冲突。**

  - 全局作用域

    作用于所有代码执行的环境(整个 script 标签内部)或者一个独立的 js 文件，在任何地方都可以访问.一般来说以下几种情形拥有全局作用域：

    1.最外层函数和在最外层函数外面定义的变量

    ```js
    var outVariable = "我是最外层变量"; //最外层变量
    function outFun() { //最外层函数
        var inVariable = "内层变量";
        function innerFun() { //内层函数
            console.log(inVariable);
        }
        innerFun();
    }
    console.log(outVariable); //我是最外层变量
    outFun(); //内层变量
    console.log(inVariable); //inVariable is not defined
    innerFun(); //innerFun is not defined
    ```

    2.所有末定义直接赋值的变量

    ```js
    function outFun2() {
        variable = "未定义直接赋值的变量";
        var inVariable2 = "内层变量2";
    }
    outFun2();//要先执行这个函数，否则根本不知道里面是啥
    console.log(variable); //未定义直接赋值的变量
    console.log(inVariable2); //inVariable2 is not defined
    ```

    3.所有window对象的属性

    **全局作用域的缺点**：如果变量都定义全局作用域中，会污染全局命名空间, 容易引起命名冲突。

  - 函数作用域

    作用于函数内的代码环境，只能在函数内部访问

    ```
    function doSomething(){
        var blogName="blog";
        function innerSay(){
            alert(blogName);
        }
        innerSay();
    }
    alert(blogName); //blogName is not defined
    innerSay(); //innerSay is not defined
    ```

    **作用域是分层的，内层作用域可以访问外层作用域的变量，反之则不行**

    **块语句，如 if 和 switch 条件语句或 for 和 while 循环语句，它们不会创建一个新的作用域**。在块语句中定义的变量将保留在它们已经存在的作用域中。

    ```
    if (true) {
        // 'if' 条件语句块不会创建一个新的作用域
        var name = 'Hammad'; // name 依然在全局作用域中
    }
    console.log(name); // logs 'Hammad'
    ```

  - 块级作用域（**ES6**）

    块级作用域在如下情况被创建：

    1. 在一个函数内部
    2. 在一个代码块（由一对花括号包裹）内部

- ##### 作用域链

  当我们查找一个变量时，如果当前执行环境中没有找到，就向外层去找同名变量，这种作用域产生的“由内向外”的过程就是作用域链。

[关于作用域和作用域链的详细介绍](https://juejin.im/post/5c8290455188257e5d0ec64f)

#### 6.原型和原型链

使用构造函数创建某一类对象

```js
function Star() {
    
}
var person = new Star();
console.log(Object.getPrototypeOf( person)); 
person.name = 'Kevin';
//Star 就是一个构造函数，我们使用 new 创建了一个实例对象 person。
```

- ##### 原型对象

  js作者在设计js的时候，希望是一门简单的编程语言，但js里面的所有数据类型都是对象（Object）,必须有一种机制，将所有对象联系起来，所以设计了“继承”。但是不想js变得复杂，没有引入“类”（Class）的概念，根据（java、c++语言的思想）引入了new命令，通过new构造函数实例化对象，同时为构造函数设置了 prototype 属性，这样所有的实例对象共享同一个prototype，从外界来看，prototype就好像是实例对象的原型对象

  每一个构造函数的内部都有一个 prototype 属性(原型对象)，prototype是一个对
  象，这个对象包含了所有实例共享的属性和方法。
  
- ##### 原型

  new的实例化对象都具有的一个`__proto__`属性(原型)，这个属性会指向构造函数的原型对象。

  `__proto__`的意义就在于为对象的查找机制提供一个方向，或者说一条路线。但是它是一个非标准属性，因此实际开发中，不可以使用这个属性。ES5 中新增了一个 Object.getPrototypeOf() 方法，我们可以通过这个方法来获取对象的原型。
  
- ##### 原型链

  当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的原型上查找，如果还没有就查找原型对象的原型，一直找到Object原型对象的原型（null）为止，这就是原型链

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200613090128300.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70)



构造函数、实例和原型对象三角关系：

1.构造函数的prototype属性指向了构造函数原型对象
2.实例对象是由构造函数创建的,实例对象的`__proto__`属性指向了构造函数的原型对象
3.原型对象的constructor属性指向了构造函数,实例对象的原型的constructor属性也指向了构造函数

#### 7.new对象是内部做了什么

（1）**创建一个新对象，并继承其构造函数的`prototype`**（继承构造函数原型对象上的属性和方法）

（2）**执行构造函数，方法内的`this`指向该对象**（执行构造函数内的赋值操作）

（3）**返回新对象**

#### 8.闭包

闭包（closure）是指能够访问另一个函数内部变量的函数。

创建闭包：在一个函数内创建另一个函数，创建的函数可以访问到当前函数的局部变量。

闭包的用途：

- 创建私有变量

  通过使用闭包，我们可以通过在函数外部调用闭包函数，从而在外部访问到函数内部的变量

- 延伸变量的作用域

  闭包函数保留了变量对象的引用，让这个变量在函数执行完毕之后不会被回收

  ```js
  function fn(){
      var n = 0;
      function add(){
         n++;
         console.log(n);
      }
      return add;
  }
  var f = fn(); //注意，函数名只是一个标识（指向函数的指针），而（）才是执行函数；
  f();    //1
  f();    //2  第二次调用n变量还在内存中
  ```

- 避免全局变量污染

闭包的缺点：

- 导致内存泄漏（不合理的使用闭包，从而导致某些变量一直被留在内存当中。）

闭包应用场景：

1. Ajax请求的成功回调

2. 事件绑定的回调方法

3. setTimeout的延时回调

4. 一个函数内部返回另一个匿名函数

#### 9.DOM事件流

事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程即 DOM 事件流。

DOM 事件流会经历3个阶段：

1. 捕获阶段

   由 DOM 最顶层节点（document）开始，然后逐级向下传播到最具体的元素接收的过程。

2. 当前目标阶段

3. 冒泡阶段

   事件开始时由最具体的元素接收，然后逐级向上传播到到 DOM 最顶层节点（document）的过程

#### 10.事件委托

事件委托也称为事件代理,利用事件冒泡机制，当子元素的事件触发，会冒泡到父元素，让父元素代替执行

**事件委托的好处**

- 只绑定一次事件，无需频繁访问DOM，性能较高
- 当有新DOM生成时，无需重复绑定事件

**事件委托的局限性**

比如 onblur、onfocus、onmouseenter、onmouseleave之类的事件本身没有事件冒泡机制，所以无法委托

事件委托的应用场景

1.实现事件的动态绑定，比如说新增了一个子节点，我们直接父元素中的监听函数处理这个子节点触发的事件

```js
const oUl = document.getElementById("ul");
let num = 4;
oUl.addEventListener('mouseover',function(e){
  let evt = e || window.event;
  let target = evt.target || evt.srcElement;
  if(target.nodeName.toLowerCase() == 'li'){
     target.style.background='red';
  }
});　　
oUl.addEventListener('mouseout',function(e){
   let evt = e || window.event;
   let target = evt.target || evt.srcElement;
   if(target.nodeName.toLowerCase() == 'li'){
       target.style.background='orange';
    }                
 });
 btn.onclick = function(){
   num++;
   let oLi = document.createElement('li');
   oLi.innerHTML = 111*num;
   oUl.appendChild(oLi);
}
```

2.实现ul中的每个li的点击打印事件，直接在ul中的监听函数处理子节点触发的事件

```
<script>
  const oUl = document.getElementById("ul");
  oUl.addEventListener('click',function(e){
     let evt = e || window.event;//e指事件对象
     let target = evt.target || evt.srcElement;//target是e对象的一个属性，可以返回事件的目标节点
     //nodeName获取具体的标签名，返回是大写的；toLowerCase()将其转换成小写
     if(target.nodeName.toLowerCase() == 'li'){
       console.log('事件委托实现li的点击事件');
     }
 })　
 
//jq方式实现相对而言简单 $(“oul”).on(“click”,“li”,function(){//事件逻辑})  
//其中第二个参数指的是触发事件的具体目标，特别是给动态添加的元素绑定事件，这个特别起作用
</script>
```

参考资料：[js中的事件委托或是事件代理详解](https://www.cnblogs.com/liugang-vip/p/5616484.html)

#### 11.DOM 操作——节点操作、获取元素

| 操作类型 | 方法                                                         | 方法的解释                                                   |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 创建节点 | **document.createElement(‘tagName’)**                        | 创建由 tagName 指定的 HTML 元素                              |
| 添加节点 | **node.appendChild(child)**                                  | 将一个节点添加到指定父节点的子节点列表末尾                   |
| 插入节点 | **node.insertBefore(child, 指定元素)**                       | 将一个节点添加到父节点的指定子节点前面                       |
| 删除节点 | node.removeChild()                                           | 从 DOM 中删除一个子节点                                      |
| 复制节点 | node.cloneNode()                                             | 返回调用该方法的节点的一个副本                               |
| 查找节点 | getElementById(‘id’)<br/>getElementsByTagName(‘标签名’)<br/>getElementsByClassName(‘类名’)<br/>querySelector('选择器')<br/>querySelectorAll('选择器') | 根据ID获取元素对象<br/>根据标签名获取元素对象<br/>根据类名返回元素对象集合<br/>根据指定选择器返回第一个元素对象<br/>根据指定选择器返回所有元素对象集合 |

#### 12.数组和对象常见的原生方法

下划线（_）：表示改变原数组的方法

| 数组方法         | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| <u>push()</u>    | 数组末尾添加一个或多个元素                                   |
| <u>unshift()</u> | 数组首位添加一个或多个元素                                   |
| <u>shift()</u>   | 删除数组的第一个元素，并返回该值                             |
| <u>pop()</u>     | 删除数组的最后一个元素，并返回该值                           |
| <u>splice()</u>  | 用于插入、删除、替换数组的元素                               |
| <u>reverse()</u> | 颠倒数组中元素的位置                                         |
| <u>sort()</u>    | 对数组元素进行排序（从小到大）                               |
| isArray()        | 用于确定传递的值是否是一个 Array，返回一个布尔值             |
| toString()       | 把数组转换成字符串，并返回结果                               |
| join()           | 所有的数组元素被转换成字符串，再用一个分隔符将这些字符串连接起来 |
| slice()          | 可从已有的数组中返回选定的元素                               |
| concat()         | 用于连接两个或多个数组                                       |
| indexOf()        | 返回指定元素在数组中的第一个索引，如果不存在，则返回-1（用于查找一个元素的位置） |
| lastindexOf()    | 返回指定元素在数组中的最后一个的索引,如果不存在，则返回-1    |
| `forEach()`      | 为每个数组元素执行一次 `callback` 函数                       |
| `map()`          | 给原数组中的每个元素都按顺序调用一次  `callback` 函数,该函数每次执行后的返回值组成一个新数组（映射函数） |
| `every()`        | 测试一个数组内的所有元素是否都能通过某个指定函数的测试。返回一个布尔值 |
| `some()`         | 测试数组中是不是至少有1个元素通过了指定函数测试。返回一个布尔值 |
| `filter()`       | 为数组中的每个元素调用一次 `callback` 函数，该函数每次执行后的结果返回 true 的元素形成一个新数组（过滤函数） |
| `includes()`     | 方法用来判断一个数组是否包含一个指定的值，返回一个布尔值(用于判断一个元素是否存在于数组中) |
| `find()`         | 返回数组中满足指定测试函数的第一个元素的**值**。否则返回 undefined |
| `findIndex()`    | 返回数组中满足指定测试函数的第一个元素的**索引**。否则返回-1 |

| 对象方法            | 说明                                                         |
| ------------------- | ------------------------------------------------------------ |
| charAt()            | 返回指定位置的字符                                           |
| charCodeAt()        | 返回在指定位置字符的Unicode 编码                             |
| concat()            | 将一个或多个字符串与原字符串连接合并，形成一个新的字符串并返回(不推荐) |
| `indexOf()`         | 返回调用它的 String对象中第一次出现的指定值的索引，如果未找到该值，则返回 -1。 |
| **`substring() `**  | 提取字符串中两个指定的索引之间的字符                         |
| `slice()`           | 提取某个字符串的一部分，并返回一个新的字符串                 |
| `split() `          | 使用指定的分隔符字符串将一个String对象分割成子字符串数组     |
| toLocaleLowerCase() | 字符串被转换为小写的格式                                     |
| toLowerCase()       | 字符串被转换为小写的格式                                     |
| toLocaleUpperCase() | 把字符串转换为大写格式                                       |

#### 12.null、undefined 与 undeclared 的区别

- ##### null

  null表示“没有对象”，即该处不应该有值

  典型用法：

  - 作为函数的参数，表示该函数的参数不是对象
  - 作为对象原型链的终点

- ##### undefined

  undefined表示“缺少值”，此处应该有一个值，只是没有定义

  典型用法：

  - 变量被声明了，但没有赋值时，就等于undefined

  - 调用函数时，应该提供的参数没有提供，该参数等于undefined

  - 对象没有赋值的属性，该属性的值为undefined

  - 函数没有返回值时，默认返回undefined

- ##### undeclared

  没有在作用域中声明过的变量，对其引用，浏览器会报引用错误，如 ReferenceError: b is not defined 。

  扩展：我们可以使用 typeof 的安全防范机制来避免报错，因为对于 undeclared（或者 not defined ）变量，typeof 会返回 "undefined"。

扩展：

null == undefined 为true，因为它们是类似的值；如果用全等于(===)，null === undefined会返回false ,因为它们是不同类型的值。

#### 13.call、apply和bind的理解

> `call` 和 `apply` 实现函数调用，并都可以改变 `this` 的指向。作用都是相同的，只是传参的方式不同。
>
> 除了第一个参数外，`call` 可以接收一个参数列表，`apply` 只接受一个参数数组。

模拟实现call()

```js
  //变更函数调用者示例
  function foo() {
      console.log(this.name)
   }            
   const obj = {
      name: '写代码'
   }
   
  //思路：  
  // 改变了 this 指向，让新的对象可以执行该函数。
  // 那么思路是否可以变成给新的对象添加一个函数，然后在执行完以后删除？      
  Function.prototype.myCall = function(thisArg,...args){
      console.log(this);
      //this不是函数，抛出异常
      if(typeof this !== 'function'){
          throw new TypeError('error')
      }
      const fn = Symbol('fn');// 声明一个独有的Symbol属性, 防止fn覆盖已有属性
      const param = thisArg || window;// 若没有传入第一个参数, 默认绑定window对象
      param[fn] = this;// 给 param 添加一个属性fn，并让this指向此对象
      const result = param[fn](...args); // // 将param后面的参数取出来，执行当前函数
      delete param[fn];// 删除我们声明的fn属性
      return result; // 返回函数执行结果
  }
  foo.myCall(obj);  //输出写代码
```

模拟实现apply()

```js
Function.prototype.myApply = function(thisArg,args){
      console.log(this);
      //this不是函数，抛出异常
      if(typeof this !== 'function'){
          throw new TypeError('error')
      }
      const fn = Symbol('fn');// 声明一个独有的Symbol属性, 防止fn覆盖已有属性
      const param = thisArg || window;// 若没有传入第一个参数, 默认绑定window对象
      param[fn] = this;// 给 param 添加一个属性fn，并让this指向此对象
      const result = param[fn](...args); // // 将param后面的参数取出来，执行当前函数
      delete param[fn];// 删除我们声明的fn属性
      return result; // 返回函数执行结果
  }
  foo.myApply(obj,[]);  //输出写代码
```

> `bind()` 方法创建一个新的函数，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。
> 语法: function.bind(thisArg, arg1, arg2, ...)

#### 14.Ajax 是什么? 如何创建一个 Ajax？

Ajax是一种异步通信的方法，通过 js 脚本向服务器发起 http 通信，然后根据服务器返回的数据，更新网页的相应部分，而不用刷新整个页面的一种方法。（异步局部刷新技术）

创建步骤：

创建对象 => 配置Ajax请求地址 => 发送请求 => 监听请求，接受响应

原生写法：

```js
//1：创建Ajax对象
var xhr = window.XMLHttpRequest?new XMLHttpRequest():new ActiveXObject('Microsoft.XMLHTTP');// 兼容IE6及以下版本
//2：配置 Ajax请求地址
xhr.open('get','url',true);
//3：发送请求
xhr.send(null); // 严谨写法
//4:监听请求，接受响应
xhr.onreadysatechange=function(){
//readystate是当前的状态值
//status是Http的状态码 200表示ok  
   if(xhr.readySate==4&&xhr.status==200)      console.log(xhr.responseText)
}
```

jQuery写法

```js
$.ajax({
 type:'post',
 url:'',
 async:ture,//async 异步  sync同步
 data:data,//针对post请求
 dataType:'json',
 success:function (msg) {},
 error:function (error) {}
})
```

promise 封装实现

```
function getJSON(url) {
  // 创建一个 promise 对象
  let promise = new Promise((resolve, reject) => {
      let xhr = new XMLHttpRequest();
      // 新建一个 http 请求
      xhr.open("GET", url, true);
      // 设置状态的监听函数
      xhr.onreadystatechange = function() {
          if (this.readyState !== 4) return;
      // 当请求成功或失败时，改变 promise 的状态
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };

    // 设置错误监听函数
    xhr.onerror = function() {
      reject(new Error(this.statusText));
    };
    // 设置响应的数据类型
    xhr.responseType = "json";
    // 设置请求头信息
    xhr.setRequestHeader("Accept", "application/json");
    // 发送 http 请求
    xhr.send(null);
  });

  return promise;
}
```

#### 15.如何实现继承

##### 一. 原型链继承

原型链继承的原理很简单，直接让**子类的原型对象指向父类实例**，当子类实例找不到对应的属性和方法时，就会往它的原型对象，也就是父类实例上找，从而**实现对父类的属性和方法的继承**

原型继承的缺点:

1.由于子类实例原型对象指向父类实例, 因此子类实例继承的引用类型如果被修改，会影响到所有的实例对象

2.创建子类实例无法向父类构造函数传参, 即没有实现`super()`的功能

##### 二. 构造函数继承

构造函数继承，即在**子类的构造函数中执行父类的构造函数**（使用call或者apply方法），并为其绑定子类的`this`，让父类的构造函数把成员属性和方法都挂到`子类的this`上去，这样既能**避免实例之间共享一个原型实例**，又能向**父类构造方法传参**

构造函数继承的缺点:

1.继承不到父类原型上的属性和方法

##### 三. 组合式继承

原型链继承和构造函数继承组合起来使用

组合式继承的缺点:

1.每次创建子类实例都执行了两次构造函数(`Parent.call()`和`new Parent()`)，虽然这并不影响对父类的继承，但子类创建实例时，原型中会存在两份相同的属性和方法，这并不优雅

##### 四. 寄生式组合继承

为了解决构造函数被执行两次的问题, 我们将`指向父类实例`改为`指向父类原型`, 减去一次构造函数的执行

寄生式组合继承的缺点:

1.由于子类原型和父类原型指向同一个对象，我们对子类原型的操作会影响到父类原型，例如给`Child.prototype`增加一个getName()方法，那么会导致`Parent.prototype`也增加或被覆盖一个getName()方法，为了解决这个问题，我们给`Parent.prototype`做一个浅拷贝 `Object.create(Parent.prototype)` 

```js
// 父类初始化实例属性和原型属性
function Parent(name){
	this.name = name
	this.hobby = ["吃饭", "睡觉"];
}
Parent.prototype.getName = function(){
	console.log(this.name)
}
function Child(name,age) {
    // 构造函数继承
    Parent.call(this, name) 
    this.age = age
}
//原型链继承
// Child.prototype = new Parent()

function inheritPrototype(Child,Parent){
 let parentProto = Object.create(Parent.prototype)//创建父类原型的一个副本
 parentProto.constructor = Child
 Child.prototype = parentProto //子类原型指向父类原型
}
// 将父类原型指向子类
inheritPrototype(Child, Parent);

// 新增子类原型属性
Child.prototype.sayAge = function(){
    console.log(this.age);
};

let instance1 = new Child("儿子1", 23);
let instance2 = new Child("儿子2", 23);
instance1.hobby.push("打豆豆");
console.log(instance1.hobby);// ["吃饭", "睡觉"，"打豆豆"]
console.log(instance2.hobby);// ["吃饭", "睡觉"]
```

> 以上四种继承方式是在ES5环境下继承的实现，具体实现过程：
>
> 一开始最容易想到的是`原型链继承`，通过把子类实例的原型指向父类实例来继承父类的属性和方法，但原型链继承的缺陷在于`对子类实例继承的引用类型的修改会影响到所有的实例对象`以及`无法向父类的构造方法传参`。
> 因此我们引入了`构造函数继承`, 通过在子类构造函数中调用父类构造函数，并传入子类this来获取父类的属性和方法，但构造函数继承也存在缺陷，构造函数继承`不能继承到父类原型链上的属性和方法`。
> 所以我们综合了两种继承的优点，提出了`组合式继承`，但组合式继承也引入了新的问题，它`每次创建子类实例都执行了两次父类构造方法`，我们通过将`子类原型指向父类实例`改为`子类原型指向父类原型的浅拷贝`来解决这一问题，也就是最终实现 —— `寄生组合式继承`

##### 五.ES6中的继承方式

用class定义类，用extends继承类，用super()表示父类

```js
创建父类
class  Father{
  constructor(name) {
   //构造器代码，new时自动执行
      this.name = name;
  }
  work(){
        console.log(this.name+'刷碗');
    }
}
 
创建子类并继承父类
class Son extends Father {
  constructor() {
   super()  //表示父类
  }
}
var son=new Son( )
son.work()
```

#### 16.ES6新增特性

常用的主要有

- ##### let/const

  - let

    1.用于声明变量的关键字

    2.let声明的变量有一个块级作用域范围

  - const

    1.用于声明一个只读的常量

    2.一旦声明必须赋值,并且声明后不能再修改
    3.如果声明的是复合类型数据，可以修改其属性

  `var`,`let`和`const`的区别是什么？

  1.var声明变量存在变量提升，let和const不存在变量提升

  2.let和const声明形成块作用域

  3.同一作用域下let和const不能声明同名变量，而var可以

- ##### 箭头函数

  ES6中使用箭头函数(=>)来定义函数，有以下特点： 

  - 当函数体中只有一个表达式或值需要返回，不需要 return 语句，箭头函数就会有一个隐式的返回。

  - 箭头函数中有一个参数，则可以省略括号

  - 箭头函数不能访问arguments对象，可以使用rest参数来获得在箭头函数中传递的所有参数

- ##### 模板字符串

  反引号和使用`${express}`嵌入一个表达式组成

- ##### 解构赋值

  在ES6中可以从数组和对象中提取值，对变量进行赋值，称为解构赋值。

  解构赋值就是只要等号两边的模式相同，左边的变量就会被对应赋值。

  解构赋值允许指定默认值。应该注意undefined，因为undefined是不能赋值的

- ##### 模块的导入(import)和导出(export default/export)

- ##### Promise

  Promise 是异步编程的一种解决方案。以同步的方式写异步的代码，避免了多层回调函数嵌套(回调地狱)问题。

  一个Promise有几种状态：

  1. pending初始状态，既不是成功状态，也不是失败状态。
  2. fulfilled表示操作成功完成。
  3. rejected表示操作失败。

  pending 状态的 Promise 对象会触发 fulfilled/rejected 状态，一旦状态改变，Promise 对象的 then 方法就会被调用；否则就会触发 catch()方法

  **Promise.all()**

  `Promise.all(iterable)`用于将多个Promise 实例包装成一个新的 Promise实例，参数为一组 Promise 实例组成的数组

  ```js
  let p1 = Promise.resolve(1);
  let p2 = Promise.resolve(2);
  let p3 = Promise.resolve(3);
  
  let p = Promise.all([p1,p2,p3]);
  
  p.then(data=>{
    console.log(data) // [1,2,3]
  }
  ------------------------------------------------
  let p1 = Promise.resolve(1)
  let p2 = Promise.resolve(2)
  let p3 = Promise.reject(3)
  
  let p = Promise.all([p1, p2, p3])
  p.then(data => {
    console.log(data)
  }).catch(err => {
    console.log("err:" + err) // 3
  })
  ```

  当 p1, p2, p3 状态都 Resolved 的时候，p 的状态才会 Resolved;

  只要有一个实例 Rejected ，此时第一个被 Rejected 的实例返回值就会传递给 P 的回调函数(只要有一个失败了就直接失败)

  **Promise.race()**

  用于将多个 Promise 实例，包装成一个新的 Promise 实例。
  `const p = Promise.race([p1, p2, p3]);`

  说明: 返回一个新的promise, 第一个完成的promise的结果状态就是最终的结果状态

- ##### async/await

  `async` 是异步的意思，而 `await` 是 `async wait`的简写，即异步等待

  从语义上理解 `async` 用于声明一个 `function` 是异步的，`await` 用于等待一个异步方法执行完成

  另外 `await` 只能出现在 `async` 函数中

  ```js
  async function test() {
    return "this is async"
  }
  const res = test()
  console.log(res)// Promise {<resolved>: "this is async"}
  ```

  `async` 函数返回的是一个 Promise 对象

  实现一个业务需求：多个请求，每个请求依赖于上一个请求的结果

  ```js
  function takeTime(n){
  	return new Promise(resolve => {
  		setTimeout(() => resolve(n+200),n)
  	})
  }
  function step1(n){
  	console.log(`step1 with ${n}`)
  	return takeTime(n)
  }
  function step2(n) {
    console.log(`step2 with ${n}`)
    return takeTime(n)
   }
  function step3(n) {
      console.log(`step3 with ${n}`)
      return takeTime(n)
  }
  -----------------------------------
  //使用Promise
  function doIt(){
      const time1 = 200;
      step1(time1)
      	.then(time2 => step2(time2))
      	.then(time3 => step3(time3))
      	.then(result => {
          	console.log(`result is ${result}`)
      })
  }
  doIt();
  -------------------------------------
  //使用async/await
  async function doIt(){
      const time1 = 300;
      const time2 = await step1(time1);
      const time3 = await step2(time2);
      const result = await step3(time3);
      console.log(`result is ${result}`);
  }
  doIt();
  ```

  优点：

  - 内置执行器

    `async` 函数自带执行器，也就是说，`async` 函数的执行，与普通函数一模一样，只要一行

  - 更好的语义

    `async` 和 `await`，比起 `*` 和 `yield`，语义更清楚了，`async` 表示函数里有异步操作，`await` 表示紧跟在后面的表达式需要等待结果

  缺点：

  滥用 `await` 可能会导致性能问题，因为 `await` 会阻塞代码，也许之后的异步代码并不依赖于前者，但仍然需要等待前者完成，导致代码失去了并发性

- 数组字符串的新方法

#### 17.从输入URL到页面加载完中间发生了什么

大致过程：

1. DNS解析

2. TCP连接

3. 发送HTTP请求

4. 服务器处理请求并返回需要的数据

5. 浏览器解析渲染页面

6. 连接结束

输入了一个域名,域名通过DNS解析,找到这个域名对应的ip地址,通过TCP连接,发送HTTP请求，WEB服务器处理请求并返回数据,浏览器根据返回数据构建DOM树,通过渲染引擎及js解析引擎将页面渲染出来,关闭tcp连接

#### 18.git相关

（1） 你的项目是如何管理的？

答：主要通过git来进行项目版本控制的

（2） 说几个git常用命令？

答：我工作中常用的有git add ,git status,git commit –m,git push,git pull等

（3） 说一下多人操作同一个文件，如果出现冲突该如何解决？

答：当遇到多人协作修改同一个文件时出现冲突，我先将远程文件先git pull下来，手动修改冲突代码后，再git add ,git commit,git push再上传到远程仓库。如果pull也pull不下来提示冲突的话，可以先通过git stash暂存下来，然后再pull拉取，然后git  stash pop，取出原来写的，手动修改，然后提交

#### 19.浅拷贝、深拷贝

- **浅拷贝：** 创建一个新对象，这个对象有着原始对象属性值的一份精确拷贝。如果属性是基本类型，拷贝的就是基本类型的值，如果属性是引用类型，拷贝的就是内存地址 ，所以如果其中一个对象改变了这个地址，就会影响到另一个对象。

  > **浅拷贝的实现方式：**
  >
  > **Object.assign() 方法**：用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。

  ```js
  let a = {
      name: "tom",
      book: {
          title: "You Don't Know JS",
          price: "25"
      }
  }
  let b = Object.assign({},a);
  console.log(b);
  a.name = "change";
  a.book.price = "55";
  console.log(a);
  console.log(b);
  ```

  > **Array.prototype.slice()：**slice() 方法返回一个新的数组对象，这一对象是一个由 begin和end（不包括end）决定的原数组的浅拷贝。原始数组不会被改变。
  >
  > 展开语法（...）

  应用场景：寄生式组合继承

- **深拷贝：** 将一个对象从内存中完整的拷贝一份出来,从堆内存中开辟一个新的区域存放新对象,且修改新对象不会影响原对象。

  > **深拷贝的实现方式：**
  >
  > JSON.parse(JSON.stringify(object)): 缺点诸多（会忽略undefined、symbol、函数；不能解决循环引用；不能处理正则、new Date()）

  ```js
  let a = {
      name: "tom",
      book: {
          title: "You Don't Know JS",
          price: "45"
      }
  }
  let b = JSON.parse(JSON.stringify(a));
  console.log(b);
  ```

  浅拷贝+递归 （只考虑了普通的 object和 array两种数据类型）

#### 20.跨域的解决方案

产生跨域的情况有：不同协议，不同域名，不同端口的域名和ip地址的访问都会产生跨域。

- 代理（前端代理和后端代理）

前端代理,在vue中主要是通过vue.config.js文件来配置的，其中有个proxyTable来配置跨域的

- CORS

CORS全称叫跨域资源共享，主要是后台工程师设置后端代码来达到前端跨域请求的

​      注：现在主流框架都是用代理和CORS跨域实现的

#### 21.JavaScript执行机制

JavaScript是一门单线程语言

**JS执行步骤**

1. **先执行**执行栈中的**同步任务**。
2. **异步任务**（回调函数）放入**任务队列**中。
3. 一旦执行栈中的所有**同步任务执行完毕**，系统就会按次序**读取**任务队列中的**异步任务**，于是被读取的异步任务结束等待状态，**进入执行栈**，开始**执行**。

事件循环(Event Loop)是js实现异步的一种方法，也是js的执行机制。

除了广义的同步任务和异步任务，我们对任务有更精细的定义：

- macro-task(宏任务)：包括整体js代码，setTimeout，setInterval

- micro-task(微任务)：Promise.then,process.nextTick

  注：

  1.一般js代码是同步的，promise也是同步执行的

  2.setTimeout，setInterval是异步的

  3.promise需要resolve或者reject才会执行then或者catch里面的内容

事件循环，宏任务，微任务的关系

1. 同步的代码会按照执行顺序执行，其它代码属于宏任务的放到宏队列，微任务放到微队列
2. 执行顺序是宏任务-微任务-宏任务……，进入整体代码(宏任务)后，开始第一次循环，接着执行所有的微任务。然后再次从宏任务开始，找到任务队列里的第一个宏任务执行完毕，再执行所有微任务。再进入任务队列里的下一个宏任务

##### 总结：（面试回答）

js是一门单线程语言。整体js代码作为第一个宏任务进入主线程中，同步代码按照执行顺序执行，其它代码划分为宏任务和微任务，放入任务队列中。一旦同步任务执行完毕，就按照宏任务=>微任务=>宏任务的执行顺序执行，因为整体js代码为宏任务，所以接着执行所有的微任务。然后再从宏任务开始，找到任务队列中第一个宏任务执行完毕，再执行所有微任务。再进入任务队列里的下一个宏任务。直到全部代码执行完毕

#### 22.垃圾回收机制

JavaScript垃圾回收机制的原理：找出不再使用的变量，然后释放掉其占用的内存，但是这个过程不是时时的，为此垃圾回收器会按照固定的时间间隔周期性的执行

- **标记清除**

  垃圾收集器先给内存中所有对象加上标记，然后从根节点开始遍历，去掉被引用的对象和运行环境中对象的标记，剩下的被标记的对象就是无法访问的等待回收的对象。

- 引用计数

  给一个变量赋引用类型值，则该值的引用次数就是1。如果又把该值赋给另一个变量，则该值引用次数加1；相反，如果包含该值的变量又取得另外一个值，则该值的引用次数减1；垃圾回收器会回收引用次数为0的值所占用的内存空间。但是当对象循环引用时，会导致引用次数永远无法归零，造成内存无法释放。