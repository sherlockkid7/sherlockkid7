---
title: Webpack
date: 2020-06-16 23:18:56
tags: Webpack
categories: Webpack
---

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200616225712689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjY4NDg2MA==,size_16,color_FFFFFF,t_70#pic_center)

## Webpack

### 1.基本概述

是一个现代的JavaScript应用的静态模块打包工具

### 2.安装依赖

1.首先需要安装Node.js（Node.js自带了软件包管理工具npm）

2.全局安装webpack：运行npm i webpack -g全局安装webpack，这样就能在全局使用webpack的命令

3.局部安装webpack：在项目根目录中运行npm i webpack --save-dev安装到项目依赖中

为什么全局安装后，还需要局部安装呢

- 在终端直接执行webpack命令，使用的全局安装的webpack（只有那些安装到全局 -g 的工具，才能在终端中正常执行）
- 当在package.json中定义了scripts时，其中包含了webpack命令，那么使用的是局部webpack
  注意：还需安装npm i webpack-cli -D

### 3.准备工作

- **npm init** 初始化项目，使用npm管理项目中的依赖包

- 创建项目基本的目录结构 dist 、src等

  文件和文件夹解析：

  - dist文件夹：用于存放之后打包的文件
  - src文件夹：用于存放我们写的源文件
    - main.js：项目的入口文件。
    - mathUtils.js：定义了一些数学工具函数，可以在其他地方引用，并且使用。
  - index.html：浏览器打开展示的首页html
  - package.json：通过npm init生成的，npm包管理的文件

- js文件的打包

  使用webpack的指令：webpack  src/js/main.js  dist/bundle.js

- 使用打包后的文件

  bundle.js文件，是webpack处理了项目直接文件依赖后生成的一个js文件，我们只需要将这个js文件在index.html中引入即可

### 4.Webpack配置

- 项目根目录中创建一个webpack.config.js文件。

- webpack.config.js中配置指定入口文件和输出文件的路径

- package.json中定义启动

  可以在package.json的scripts中定义自己的执行脚本。
  `"dev": "webpack"`
  执行命令 ： npm run dev
  package.json中的scripts的脚本在执行时，会按照一定的顺序寻找命令对应的位置。首先，会寻找本地的node_modules/.bin路径中对应的命令。如果没有找到，会去全局的环境变量中寻找。

### 5.loader的使用

#### 5.1loader使用过程

步骤一：通过npm安装需要使用的loader
步骤二：在webpack.config.js中的modules关键字下进行配置

#### 5.2css文件处理:

 1.安装 cnpm i style-loader css-loader --save-dev                                                                

 2.修改webpack.config.js这个配置文件：

```js
module: { // 用来配置第三方loader模块的
        rules: [ // 文件的匹配规则
            { test: /\.css$/, use: ['style-loader', 'css-loader'] }//处理css文件的规则
        ]
    }
```

注意：

- use表示使用哪些模块来处理test所匹配到的文件；
- use中相关loader模块的调用顺序是从后向前调用的；
- css-loader只负责加载css文件
- style-loader负责将css具体样式嵌入到文档中

#### 5.3less文件处理                                                    

1.安装运行cnpm i less-loader less -D(-D是--save-dev的简写)
2.修改webpack.config.js这个配置文件：

```js
{ test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] }
```

注意：这里还安装了less，因为webpack会使用less对less文件进行编译

#### 5.4sass文件处理

1.安装 cnpm i sass-loader node-sass --save-dev
2.在webpack.config.js中添加处理sass文件的loader模块：

```js
{ test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] }
```

注意：有时候使用npm i node-sass -D装不上，这时候，就必须使用 cnpm i node-sass -D

#### 5.5图片文件处理 （url-loader）

1.cnpm i url-loader file-loader --save-dev
2.在webpack.config.js中添加loader模块：

```js
{ 
    test: /\.(png|jpg|gif)$/, 
    use: [
        {
        loader:'url-loader' ,
    	option:{
            limit:8196,
            name:'img/[name].[hash:8].[ext]'
            //可以在options中添加上如下选项:
            //img：文件要打包到的文件夹
            //name：获取图片原来的名字，放在该位置
            //hash:8：为了防止图片名称冲突，依然使用hash，但是我们只保留8位
            //ext：使用图片原来的扩展名
        }
        ]
}
```

- 可以通过limit指定进行base64编码的图片大小；只有小于指定字节（byte）的图片才会进行base64编码

#### 5.6ES6语法处理(js兼容性处理)

1.安装
npm i babel-loader @babel/core core-js -D
npm i @babel/preset-env -D            

2.配置

```js
 { 
	test: /\.js$/, 
	loader: 'babel-loader', 
	//排除 node_modules 目录
	exclude: /node_modules/ ,
	options:
	//预设：指示babel做怎么样的兼容性处理
	{
	presets:[
	'@babel/preset-env'，						    
	{
		//按需加载
		useBuiltIns:'usage',
		//指定corejs版本
		corejs{
		version:3
		},
		//指定兼容性做到哪个版本浏览器
		targets{
			chrome:'60',
			firefix:'60',
			ie:'9',
			safari:'10',
			edge:'17'
		}
	}
	]
}
}
```

#### 5.7.vue文件封装处理

1.安装vue-loader和vue-template-compiler
npm install vue-loader vue-template-compiler --save-dev
2.修改webpack.config.js的配置文件：

```js
{
	test:/\.vue$/,
	use:['vue-loader']
}
```

### 6.plugin

#### 6.1基本介绍

plugin是插件的意思，通常是用于对某个现有的架构进行扩展。
webpack中的插件，就是对webpack现有功能的各种扩展，比如打包优化，文件压缩等等。

#### 6.2使用过程

步骤一：通过npm安装需要使用的plugins(某些webpack已经内置的插件不需要安装)
步骤二：在webpack.config.js中的plugins中配置插件。

#### 6.3添加版权的Plugin

该插件名字叫BannerPlugin，属于webpack自带的插件。

按照下面的方式来修改webpack.config.js的文件：

```js
plugins:[
	new webpack.BannerPlugin('最终版权归某某人所有')
]
```

#### 6.4打包html的plugin:HtmlWebpackPlugin插件

HtmlWebpackPlugin插件可以为我们做这些事情：

- 自动生成一个index.html文件(可以指定模板来生成)
- 将打包的js文件，自动通过script标签插入到body中

1.安装HtmlWebpackPlugin插件
npm install html-webpack-plugin --save-dev
2.使用插件，修改webpack.config.js文件中plugins部分的内容如下：

- 这里的template表示根据什么模板来生成index.html

- 另外，我们需要删除之前在output中添加的publicPath属性,否则插入的script标签中的src可能会有问题

  ```js
  plugins:[
  	new HtmlWebpackPlugin({
  		template:'./src/index.html'
  	})
  ]
  ```

### 7.本地开发服务器(devserver)

实现代码实时打包编译，当修改代码之后，会自动进行打包构建，浏览器自动刷新。
特点：只会在内存中编译打包，不会有任何输出

1.运行cnpm i webpack-dev-server --save-dev安装到开发依赖

2.devserver也是作为webpack中的一个选项，选项本身可以设置如下属性：

- contentBase：为哪一个文件夹提供本地服务，默认是根文件夹，我们这里要填写./dist
- port：端口号
- historyApiFallback：在SPA页面中，依赖HTML5的history模式

3.webpack.config.js文件配置修改如下：
我们可以再配置另外一个scripts：--open参数表示直接打开浏览器

### 8.示例

```js
// webpack是基于Node进行构建的，webpack的配置文件中，任何合法的Node代码都是支持的
// const path = require('path');

// 如果要配置插件，需要在导出的对象中，挂载一个 plugins 节点
const HtmlWebpackPlugin = require('html-webpack-plugin');
//提取js中的css成单独文件
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCssAssetsWebpackPlugin = require ('optimize-css-assets-webpack-plugin');
// const VueLoaderPlugin = require('vue-loader/lib/plugin');
// const webpack = require('webpack');

// 设置nodejs环境变量:决定使用browserslist的那个环境
// process.env.NODE_ENV ='development';

// 复用loader
const commentCSSLoader = [
    MiniCssExtractPlugin.loader, 
    'css-loader',
    {
        // 需要在package.json中定义browserslist
        /*
        "browserslist": {
            "development": [
              "last 1 chrome version",
              "last 1 firefox version",
              "last 1 safari version"
            ],
            "production": [
              ">0.2%",
              "not dead",
              "not op_mini all"
            ]
          }*/
		//css的兼容性处理 npm i postcss-loader postcss-preset-env -D        
        loader:'postcss-loader',
        options:{
            ident:'postcss',
            plugins:()=>[
                require('postcss-preset-env')()
            ]
        } 
    }
]
module.exports = {
    // mode:'production',
    mode:'development',
    entry: './src/js/main.js', // 入口文件
    output:{
        filename:'js/bundle.js',
        path: __dirname + '/build'
    },
    module:{
        rules:[
            { 
                test: /\.css$/, 
                use: [...commentCSSLoader] 
            },
            { 
                test: /\.less$/, 
                use: [...commentCSSLoader,
                    'less-loader'] 
            }, 
            { 
                test: /\.(jpg|png|gif|bmp|jpeg)$/,
                use: {
                    loader:'url-loader',
                    options:{
                        esModule: false,
                        limit: 10240,
                        name:'[name].[ext]-[hash:8]',
                        outputPath:'imgs'
                    }
                }
            }, // 处理图片路径的loader
            {
                test:/\.html$/,
                loader:'html-loader'
            },
            { 
                test: /\.(ttf|eot|svg|woff|woff2)$/, 
                use: 'url-loader' ,
                outputPath:'font'
            },
            //自己写的js语法检查
//npm i eslint-loader eslint eslint-config-airbnb-base eslint-plugin-import -D
            {
                // 在package.json中配置eslintConfig
                test:/\.js$/,
                exclude:/node_modules/,
                // 优先执行
                enforce:'pre',
                loader:'eslint-loader',
                options:{
                    // 自动修复eslint的错误
                    fix:true
                }
            },
            {
                test:/\.js$/,
                exclude:/node_modules/,
                loader:'babel-loader',
                options:{
                    presets:[
                        [
                            '@babel/preset-env',
                            {
                                useBuiltIns:'usage',
                                corejs:{version:3},
                                targets:{
                                    chrome:'60',
                                    firefox:'60',
                                    ie:'9',
                                    safari:'10',
                                    edge:'17'
                                }

                            }
                        ]
                    ]
                }
            }
            
    ]
    },
    plugins:[
        new HtmlWebpackPlugin({
            template:'./src/index.html',
            minify:{
                collapseWhitespace:true,
                removeComments:true
            }
        }),
        new MiniCssExtractPlugin({
            filename:'css/built.css'
        }),
        new OptimizeCssAssetsWebpackPlugin(),
        // new VueLoaderPlugin(),
        // new webpack.HotModuleReplacementPlugin()
    ],
    // resolve: {
    //     alias: { // 修改Vue被导入时包的路径
    //       "vue$": "vue/dist/vue.js"
    //     }
    // },
    devServer: {
        open:true,
        port:3000,
        compress:true,
        historyApiFallback: true,  
    }
}
```

