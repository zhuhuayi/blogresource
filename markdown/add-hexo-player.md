---
title: add-hexo-player
date: 2020-03-31 20:32:55
tags: music
---

# hexo安装player插件

hexo palyer插件[github地址](https://github.com/MoePlayer/hexo-tag-aplayer/blob/master/docs/README-zh_cn.md)

1. 在博客根目录执行

   ```shell
   npm install --save hexo-tag-aplayer
   ```

2.  模板命令

   ```js
   {% aplayer title author url [picture_url, narrow, autoplay, width:xxx, lrc:xxx] %}
   ```

3.  如何获得音乐链接

使用音乐连接：
https://music.163.com/#/song?id=1411827517

url使用下面（替换id即可）：

http://music.163.com/song/media/outer/url?id=1411827517.mp3

4.  使用代码

   ```Code
   {% aplayer "December Composer" "Vicky" "http://music.163.com/song/media/outer/url?id=1411827517.mp3" "http://p2.music.126.net/hLV8pctL418DanLsOLNCZg==/109951164577750970.jpg?param=130y130" %}
   ```



{% aplayer "December Composer" "Vicky" "http://music.163.com/song/media/outer/url?id=1411827517.mp3" "http://p2.music.126.net/hLV8pctL418DanLsOLNCZg==/109951164577750970.jpg?param=130y130" "autoplay" %}