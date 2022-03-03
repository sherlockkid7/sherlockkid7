---
title: 使用hexo+github搭建个人博客
date: 2020-05-27 15:22:10
index_img: /img/hexo.png
tags: [hexo,github]
categories: hexo
---

Hexo 是基于 Node.js 开发的一个静态博客生成器，提供本地实时预览及部署功能。

### 1.安装node和npm (或者cnpm)

### 2.安装git

### 3.安装hexo

前两步我之前已经安装过 ，就不详细记述了

**3.1 全局安装hexo**

```
npm install -g hexo-cli
```

**3.2 安装 Hexo 完成后，在指定的目录执行下列命令，Hexo 将会指定的文件夹中新建所需要的文件**

```
hexo init blog
```

**3.3 在指定文件夹下，启动本地预览服务**

```bash
cd blog
# 启动本地预览服务，默认是 127.0.0.1:4000（简写hexo s）
hexo server
```

也可以参考 Hexo 官方文档：https://hexo.io/zh-cn/ , 里面有具体的使用方式。

### 4.注册githup账号并新建仓库

新建一个名为`你的用户名.github.io`的仓库（必须是你的用户名，其它名称无效），将来个人博客访问地址就是 [http://用户名.github.io](http://test.github.io/) 

### 5.自动发布 Hexo 搭建的静态博客

**5.1先修改 `_config.yml` 配置文件**

下面是一个示例：

```yml
deploy:
  type: git
  repo: https://github用户名:密码@github.com/sherlockkid7/sherlockkid7.github.io.git
```

上面的配置选项中，一定要注意在 repo 中按照对应的格式加入 Github 用户名和密码。

**5.2安装自动发布的插件**

```bash
npm install hexo-deployer-git --save
```

**5.3使用命令一键进行发布**

```bash
hexo generate --deploy
# 或者
hexo deploy --generate
```

上面两条命令都可以，发布可能有延时，稍微等待即可。

### 6.新建文章

```
hexo new 文章标题
```

### 7.修改默认hexo主题

在github下载hexo-theme-fluid-1.8.0主题，并解压到themes文件夹中，修改 `_config.yml` 配置文件(注主题文件可以重命名为fluid)

```yml
theme: fluid
```



