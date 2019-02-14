---
author: lizzhang
comments: true
date: 2018-03-23
layout: post
slug: blog_start
title: bootstrap3 ie兼容
categories:
- Journey
tags:
- 记录
description: bootstrap3 ie兼容
---

# bootstrap3 ie兼容


### 1.这2个兼容js不被识别，然后莫名就被识别了，技术是不讲道理的
-------------
<script src="https://cdn.bootcss.com/html5shiv/r29/html5.js"></script>
<script src="https://cdn.bootcss.com/respond.js/1.4.2/respond.js"></script>

### 2.ie hack 
-------------
#### 2-1 不支持box-sizing:border-box，重新定义width;
```
    .col-xs-2{*width:14.66666% !important;}
    .col-xs-3{*width:22% !important;}
    .col-xs-5{*width:36.6666% !important;}。
```
#### 2-2 不支持display:inline-block;
```
    *display:inline;
    *zoom:1;
```
#### 2-3 z-index被下面的幻灯片遮挡;
```
     /*幻灯片同父级加上以下代码*/
    position: relative; z-index: 1;
```    
#### 2-3 ie7背景图片不根据div宽缩放
