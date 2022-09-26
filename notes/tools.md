---
layout: page
title: Some Notes of Tools
description: >
  这里记录了一些小工具的快速实现命令和问题解决方案
hide_description: false
sitemap: false
---

1. 列表（占位作用）
{:toc}

## linux基本命令
### human-readable size
~~~shell
ls -hl [filename]
~~~

## jekyll框架模板使用
### 本地网页调试
~~~shell
Bundle install
#add webrick用于处理bundler: failed to load command: jekyll问题
Bundle add webrick
Bundle exec Jekyll serve
~~~

## 科学上网
### shell设置
ShadowsocksX-NG、v2rayU等客户端工具的全局或PAC模式只能用于浏览器, 要在shell中实现翻墙需要将监听端口信息（这里为1087）写入环境变量

~~~shell
# file: `.zshrc`
export http_proxy=http://127.0.0.1:1087
export https_proxy=http://127.0.0.1:1087
~~~

## 文件乱码处理
### csv中文乱码
~~~shell
iconv -f UTF-8 -t GB18030 a.csv >b.csv
~~~

## Image Magic 
### mac无法执行display
需要重新安装imagemagick和xquartz, 来自[homebrew-imagemagick-x11](https://github.com/tlk/homebrew-imagemagick-x11)
~~~shell
brew uninstall imagemagick # without X11 support
brew install --cask xquartz
brew install tlk/imagemagick-x11/imagemagick --with-graphviz
~~~
### 查看图片大小分辨率等信息
~~~shell
identify icrar_pic.jpeg
~~~
### 缩小图片并切割同时转移重心
~~~shell
convert icrar_pic.jpeg -resize 500x500 -gravity center -crop 256x256+0-85 input.png
#注1:resize并不会强行改变比例
#注2:(0,-85)为从左上角开始的横向和纵向裁剪位置
~~~
### 从中心切割为圆形
~~~shell	
convert input.png '(' +clone -alpha transparent -draw 'circle 150,150 150,0' ')' -compose copyopacity -composite out.png
~~~
### 制作gif
~~~shell
convert -delay 20 F_*.png -loop 0 movie.gif
#注:-loop 0代表"play forever"
convert -delay 20 F_*.png -loop 0 miff:- | convert - animation.avi
#可以转换成其他格式播放
~~~
### 模糊图像
~~~shell
convert source.png -channel RGBA -blur 0x8 blurred.png
~~~

Continue with [Python Plots](notes_plot.md){:.heading.flip-title}
{:.read-more}

Back to [Some Notes](README.md){:.heading.flip-title}

