---
author: lizzhang
comments: true
date: 2018-04-12
layout: post
slug: blog_start
title: css样式问题
categories:
- Journey
tags:
- 记录
description: css样式问题
---

# css样式问题
-------------


### 1. css 样式 

##### 1-1. 插入不同大小图标后面字体不同下沉位
```
      html：     
      <div class="user-info clearfix">
         <span class="pull-left"><i class="icon_user"></i> 美腻的坑队友狂魔
         </span>
         <span class="pull-right"><i class="icon_tip"></i> 预览要求</span>
       </div>
      css：
      .icon_user,.icon_tip{background: url('../images/icon.png') no-repeat; display:inline-block; position:relative; top:3px;}
      .icon_user{width:22px; height:22px; background-position:-31px -348px; top:5px;}
      .icon_tip{width:14px; height: 14px; background-position:-31px -5px ;}
```      
##### 解决（不是特别友好）：
```
     .templet_container .user-info .pull-left{position: relative; top:-2px;}
```