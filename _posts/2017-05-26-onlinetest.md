---
author: lizzhang
comments: true
date: 2017-05-26
layout: post
slug: onlinetest
title: 在线测试小游戏-spa单页应用
categories:
- Journey
tags:
- 项目总结
description: spa单页应用移动端在线测试小游戏，首屏加载动画（音效、图片预加载）提高流畅的用户体验，转入配合css3动画和绚丽的引导页，3秒钟载入游戏介绍页配合动感的背景音乐，点击游戏按钮进入游戏页面json问题数据问题载入更加流畅、针对不同的选择，引导用户进入下一题、失败、成功、页面。配合微信sdk分享。更好友善可爱。
---
# 财富测试游戏：

### 需求：
----
>##### 页面
* 加载页Loding
* 展示页面welcome
* 活动介绍页面intro
* 测试题目展示页面question
* 结果弹出：msg
* 分享
>##### 微信功能
* 微信js-sdk 分享


### 项目流程
----
```
graph TB
A(Lading页面)-->B(welcome页面)
B(welcome页面)-->C(intro页面)
C(intro页面)-->D(question页面)
D(question页面)-->|成功|E[成功信息]
D(question页面)-->|失败|F[失败信息]
D(question页面)-->|下一题|G[下一题]
G[下一题]-->|下一题|D(question页面)

```
* loading页面（页面图片加载完毕）
* welcome页面（3秒跳转/点击关闭跳转）
* 游戏说明页面(点击按钮)
* 题目展示（选择答案）判断
* 成功页:成功信息、图片、音效
* 失败页：失败信息、图片、音效
* 下一题：本题分析、下一题按钮：回到题目展示页


### 使用的技术：
----
* html5：spa单页应用
* css：手机端reset.css/手机端页面自适应实现
* js:zepto
* json 数据存储
* php 主要用于微信


### 功能点以及实现（及遇到的坑）：
----
1. #### js-sdk（js结合php实现）
- ajax+php实现：$.ajax请求->信息获取php（微信服务器交互数据获取、json格式存储、数据封装）->ajax数据返回->调用

2. #### 数据存数：json文件 （ajax+php）
- 只涉及读取数据：
  ajax直接读取数据:使用$.getHSON()方法


3. #### 手机端检测:使用的是zepto，电脑端进行判断提示

```
var isPc = /macintosh|window/.test(navigator.userAgent.toLowerCase());
    if (isPc) {
        $('body').html('请在手机端查看效果更佳!(∩_∩)');
        return;
    }
```

4. #### 页面loading动画
   音效预加载（见6）、图片预加载（见5）后关闭loading动画。
5. #### 图片预加载：

```
function preLoadImages(arr) {
                var newimages = [],
                loadedimages = 0;
            var postaction = function() {} //此处增加了一个postaction函数
            var arr = (typeof arr != "object") ? [arr] : arr

            function imageloadpost() {
                loadedimages++;
                if (loadedimages == arr.length) {
                    postaction(newimages) //加载完成用我们调用postaction函数并将newimages数组做为参数传递进去
                }
            }
            for (var i = 0; i < arr.length; i++) {
                newimages[i] = new Image()
                newimages[i].src = arr[i]
                newimages[i].onload = function() {
                    imageloadpost()
                }
                newimages[i].onerror = function() {
                    imageloadpost()
                }
            }
            return { //此处返回一个空白对象的done方法
                done: function(f) {
                    postaction = f || postaction
                }
            }
        }
```



6. #### 背景音乐：
> 坑1：手机端默认不允许默认play()

解决：
 ```
document.addEventListener("WeixinJSBridgeReady", function() {
            toggleplay(audio, 1); //背景音乐初始化；
            PreLoadHtml();
        }, false);

   2. function toggleplay(obj, voc) {
        voc = voc ? 0 : 1;
        if (obj.paused) {
            if (obj.id === 'bgaudio') {
                audiobox.addClass('rotateall');
            }
            obj.play();
            obj.volume = voc;

        } else {
            if (obj.id === 'bgaudio') {
                audiobox.removeClass('rotateall');
            }
            obj.pause();
        }
    }
```
- 坑2：下一题的音效要第二次点击才能出现

  解决：播放音效前加载
```
function initVoice() {
        var voicelist = document.querySelectorAll(".voice");
        voicelist.forEach(function(item) {
            item.load();
        })
    }
```






6. #### ==问题页面解决==：
流程：

      1.初始化
          - income：初始金额
          - qStart："q_1_1" ：第一题
    2.绑定问题
          1.克隆question的HTML模板
          2.数据绑定（根据showQues）：
                   - html数据绑定：金额、题目、 答案
                   - 题目对应的下一题信息绑定：result
          3.事件绑定
                  1.绑定选择状态
                  2.根据选择获取下一题的信息
                  3.根据下一题信息显示不同结果：成功/失败/下一题
                       1.绑定结果信息、绑定信息对应的事件
                           成功/失败：给出原因、重新开始、分享按钮
                           下一题：给出分析、下一题按钮->重新绑定问题
                        2.显示结果信息




6. #### 其他小问题：
>##### 6.1安卓window.loacation 兼容问题

```
window.location.href = 'http://' + location.hostname + '' + location.pathname + '?time=' + ((new Date()).getTime());
```


>##### 6.2重复点击bug

```
//关闭点击按钮
            function quesOffClick(items) {
                items.off("tap");
            }
```



