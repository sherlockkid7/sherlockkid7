---
title: Vue组件化开发——学习笔记二
date: 2020-05-31 19:38:30
tags: [vue]
categories: Vue
---

### 组件化基本介绍

#### 组件化

1. 将一个完整的页面分成很多个组件。
2. 每个组件都用于实现页面的一个功能块。
3. 而每一个组件又可以进行细分。

#### **Vue组件化思想**

- 提供了一种抽象，让我们可以开发出一个个独立可复用的小组件来构造我们的应用。
- 任何的应用都会被抽象成一颗组件树。
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200530111834826.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)

组件化思想的应用：

- 尽可能的将页面拆分成一个个小的、可复用的组件。
- 让我们的代码更加方便组织和管理，并且扩展性也更强。

### **注册组件**

#### 组件的使用分成三个步骤

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200530112014942.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)

**1.创建组件构造器**

**Vue.extend()：**

1. 调用Vue.extend()创建的是一个组件构造器。 
2. 通常在创建组件构造器时，传入template代表我们自定义组件的模板。
3. 该模板就是在使用到组件的地方，显示HTML代码。

**2.注册组件**

**Vue.component()：**

调用Vue.component()是将组件构造器注册为一个组件，并且给它起一个组件的标签名称。

需要传递两个参数：1、注册组件的标签名 2、组件构造器

**3.使用组件**

组件必须挂载在某个Vue实例下，否则它不会生效。

#### 注册组件语法糖

主要是省去了调用Vue.extend()的步骤，而是可以直接使用一个对象来代替。

#### 模板的分离写法

Vue提供了两种方案来定义HTML模块内容：

- 使用```<script> ```标签
- 使用```<template>```标签

### **全局组件和局部组件**

**全局组件**

调用Vue.component()注册组件时，组件的注册是全局的,意味着该组件可以在任意Vue示例下使用

**局部组件**

如果我们注册的组件是挂载在某个实例中, 那么就是一个局部组件

### **父组件和子组件**

#### 父子组件的通信

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200530112106840.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)

**1.父子组件间传递数据**

- **父组件向子组件传递（通过props）**

  在组件中，使用选项props来声明需要从父级接收到的数据

  props的值有两种方式：

  方式一：字符串数组，数组中的字符串就是传递时的名称。

  方式二：对象，对象可以设置传递时的类型，也可以设置默认值等

  **props数据验证**

  当需要对props进行类型等验证时，就需要使用对象写法。

  验证都支持哪些数据类型呢？

  - String

  - Number

  - Boolean

  - Array

  - Object

  - Date

  - Function

  - Symbol

  - 自定义函数

    ```js
    Vue.component('my-component', {
      props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }  
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
    }
    })
    ```

注意这些 prop 会在一个组件实例创建之前进行验证，所以实例的 property (如 data、computed 等) 在 default 或 validator 函数中是不可用的。

- **子组件向父组件传递（自定义事件）**

​    自定义事件的流程：

1. 在**子组件**中，通过**$emit()**来触发事件。
2. 在**父组件**中，通过**v-on**来监听子组件事件。

**2.父子组件的访问方式**

- **父组件访问子组件（使用$children或$refs）**

  $children的访问

  this.$children是一个数组类型，它包含所有子组件对象。

  $children的**缺陷**：

  1. 通过**$children**访问子组件时，是一个**数组类型**，**访问其中的子组件**必须通过**索引值**。
  2. 但是当子组件过多，我们需要拿到其中一个时，往往不能确定它的索引值，甚至还可能会发生变化。
  3. 有时候，我们想明确获取其中一个**特定的组件**，这个时候就可以使用**$refs**

  **$refs的使用**：

  - $refs和**ref指令**通常是一起使用的。
  - 首先，我们通过ref给某一个子组件绑定一个特定的ID。
  - 其次，通过**this.$refs.ID**就可以访问到该组件了。

- **子组件访问父组件（使用$parent）**

  真实开发中尽量不要使用

### 组件数据存放

**组件中不能直接访问Vue实例中的data数据**

组件自己的数据存放在哪里呢?

- 组件对象也有一个data属性(也可以有methods等属性)
- 只是这个data属性必须是一个**函数**
- 而且这个函数需要**返回**一个**对象**，对象内部保存着数据

为什么data在组件中必须是一个函数呢?

首先，如果不是一个函数，Vue直接就会报错。

其次，原因是在于Vue让每个组件对象都返回一个新的对象，因为如果是同一个对象的，组件在多次使用后会相互影响。

### 插槽slot

#### **编译作用域**

官方给出了一条准则：父组件模板的所有东西都会在父级作用域内编译；子组件模板的所有东西都会在子级作用域内编译。

#### **基本介绍**

组件的插槽：

- 组件的插槽也是为了让我们封装的组件更加具有扩展性。
- 让使用者可以决定组件内部到底展示什么内容。

#### **基本使用**

如何使用slot？

在子组件中，使用特殊的元素```<slot>```就可以为子组件开启一个插槽。

该插槽插入什么内容取决于父组件如何使用

#### **slot分类**

**1.具名插槽slot**

如何使用具名插槽呢？

非常简单，只要给slot元素一个name属性即可

```html
<slot name='myslot'></slot>
```

**2.作用域插槽**

**父组件替换插槽的标签，但是内容由子组件来提供**

可以将 user 作为 ```<slot>``` 元素的一个 attribute 绑定上去，绑定在 ```<slot> ```元素上的 attribute 被称为插槽 prop。

```html
<span>
  <slot v-bind:user="user">
  {{ user.lastName }}
  </slot>
</span>
```

现在在父级作用域中，我们可以使用带值的 v-slot 来定义我们提供的插槽 prop 的名字


```html
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```

在这个例子中，我们选择将包含所有插槽 prop 的对象命名为 slotProps，但你也可以使用任意你喜欢的名字