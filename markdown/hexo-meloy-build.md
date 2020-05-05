---
title: hexo-meloy-build
date: 2020-03-29 16:55:13
tags: note
---

# Hexo-meloy blog搭建

## 1. 环境准备（nodejs,git）

hexo  需要用到nodejs的npm的功能，并且如果需要换主题的话，需要从github上下载对应的主题

[参考Hexo官网](https://hexo.io/zh-cn/docs/)

### 1. node安装[^1]

以linux系统举例：

```shell
# 将 node安装到 /node目录下面， 可以安装到任何你想安装的目录
mkdir /node
cd /node
wget https://nodejs.org/dist/v12.16.1/node-v12.16.1-linux-x64.tar.xz

# 该上面步骤结束后 生成node-v12.16.1-linux-x64.tar.xz
tar xf node-v12.16.1-linux-x64.tar.xz
# 生成node-v12.16.1-linux-x64文件夹

cd node-v12.16.1-linux-x64/bin/
./node -v
```

![node-version](http://resource.ubbetter.com.cn/image/202003/node-bin-version.png)

我们可以看到node的两个核心命令就在bin目录下，但是不能在任何地方执行它，所以要加上软连接，让它们在任何地方可以执行

```shell
ln -s /node/node-v12.16.1-linux-x64/bin/node /usr/local/bin
ln -s /node/node-v12.16.1-linux-x64/bin/npm /usr/local/bin
```



node安装完成

--------

### 2. git安装

最简单的安装方法，是Hexo官网安装步骤有提供的（不同linux版本，命令不一样）

centos可以参考下面命令

`sudo yum install git-core`

## 2. hexo初体验

`npm install hexo-cli -g`

执行完这个命令后，node会下载hexo 的 modle到它下面的lib/node_module/hexo-cli

运行hexo 需要hexo命令，在/node/node-v12.16.1-linux-x64/lib/node_module/hexo-cli/bin/目录下，同样我们需要全局用到这个命令，所以要建立软连接

`ln -s node/node-v12.16.1-linux-x64/lib/node_module/hexo-cli/bin/hexo  /usr/local/bin/hexo `

### 1. 初始化Blog

- 准备一个blog的目录(mkdir ...)

- 初始化blog(init)

- 启动blog

  ```shell
  mkdir /myblog
  cd /myblog
  
  #初始化
  hexo init
  
  #启动blog 默认4000端口，并且shell 关闭，或者ctrl+c，进程就会结束
  hexo s
  #让hexo一直在后台运行，可以用下面命令--经测试偶尔有用
  hexo s &
  #建议使用PM2管理进程，可以后台运行hexo
  
  ```

hexo blog创建完目录结构是这样的：

<img src="http://resource.ubbetter.com.cn/image/202003/blog-structure.png" style="zoom:80%;" />

### 2. 主题选定

[官网主题](https://hexo.io/themes/)有很多，大家可以根据自己的喜好，下载对应的主题，网上也有其他渠道有不错的Hexo主题推荐

下面我已我选择的hexo-theme-melody[^2]主题进行主题设置

```shell
cd /myblog
git clone -b master https://github.com/Molunerfinn/hexo-theme-melody themes/melody

vi _config.yml
#改theme 值为melody, 冒号后面一定要有空格，再写值
theme: melody

# 安装渲染器
npm install hexo-renderer-pug hexo-renderer-stylus --save

#推荐把主题默认的配置文件themes/melody/_config.yml复制到 hexo 工作目录下的source/_data/melody.yml，如果source/_data的目录不存在那就创建一个，后面修改菜单，主题特性，都在melody.yml修改

#command-list 然后执行,这个是后面常用的三个命令，清缓存/生成静态文件/部署到服务器/启动服务器
hexo clean
hexo g
hexo d
hexo s

```

## 3. 个性化Blog

### 1. 修改blog名称, 描述，作者

​	修改 /myblog/_config.yml

​	语言的话默认为en, 提供中文，需要修改 language: zh-Hans

<img src="http://resource.ubbetter.com.cn/image/202003/change-hexo-site.png" alt="config.yml" style="zoom:80%;" />

### 2. 修改菜单

​	修改 /myblog/source/_data/melody.yml

​	<img src="http://resource.ubbetter.com.cn/image/202003/change-melody-menu.png" alt="melody.yml" style="zoom:80%;" />

​	基本的轮廓就出来了

<img src="http://resource.ubbetter.com.cn/image/202003/blog-home.png" alt="home" style="zoom:67%;" />

其他更多的特性参考hexo-theme-melody官网，这个主题也有许多新特性一直更新\

### 3. 利用PM2管理hexo进程

​	然后后台一直运行hexo

+ 写一个start_run.js
+ pm2 start start_run.js

参考链接：

[^1]: https://blog.csdn.net/ccccc1900/article/details/104961548/
[^2]: https://molunerfinn.com/hexo-theme-melody-doc/zh-Hans/