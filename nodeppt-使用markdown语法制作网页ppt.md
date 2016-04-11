title: "nodeppt:使用markdown语法制作网页ppt"
date: 2015-12-26 09:57:58
tags: 网页ppt nodeppt
categories: 工具

---
## 为什么选择nodePPT

**这可能是迄今为止最好的网页版演示库**

 * 基于GFM的markdown语法编写
 * 支持[html混排](#mixed-code)，再复杂的demo也可以做！
 * 支持多个皮肤：[colors](http://qdemo.sinaapp.com/?theme=colors)-[moon](http://qdemo.sinaapp.com/?theme=moon)-[blue](http://qdemo.sinaapp.com/?theme=blue)-[dark](http://qdemo.sinaapp.com/?theme=dark)-[green](http://qdemo.sinaapp.com/?theme=green)-[light](http://qdemo.sinaapp.com/?theme=light)
 * 实现watch功能`nodeppt start -w`
 * 支持[20种转场动画](#transition)，可以设置单页动画
 * 支持单页背景图片
 * 多种模式：overview模式，[双屏模式](#postmessage)，[socket远程控制](#socket)，摇一摇换页，使用ipad/iphone控制翻页更酷哦~
 * 可以使用画板，**双屏同步画板**内容！可以使用note做备注
 * 支持语法高亮，自由选择[highlight样式](https://highlightjs.org/)
 * 可以单页ppt内部动画，单步动画
 * [支持进入/退出回调](#callback)，做在线demo很方便
 * 支持事件update函数，查看[demo](http://qdemo.sinaapp.com/#12)
 * zoom.js：alt+click

## demo
 * http://qdemo.sinaapp.com/
 * 多套皮肤：[colors](http://qdemo.sinaapp.com/?theme=color)-[moon](http://qdemo.sinaapp.com/?theme=moon)-[blue](http://qdemo.sinaapp.com/?theme=blue)-[dark](http://qdemo.sinaapp.com/?theme=dark)-[green](http://qdemo.sinaapp.com/?theme=green)-[light](http://qdemo.sinaapp.com/?theme=light)
 * 双屏控制：http://qdemo.sinaapp.com/?_multiscreen=1 记得允许弹窗哦~
 * 三水清的分享：http://js8.in/slide
 * 打印页面：http://qdemo.sinaapp.com/?print=1
 
## 使用

### 安装

```
npm install -g nodeppt
```

### 启动

```
nodeppt start -p port
```
OR

```
nodeppt start -p port -d path/for/ppts
```
### 创建

```
nodeppt create ppt-name
```
按照提示输入基本信息后就可以创建，默认创建是markdown版本

### 基本配置

```
title: 这是演讲的题目
speaker: 演讲者名字
url: 可以设置链接
transition: 转场效果，例如：zoomin
files: 引入js和css的地址，如果有的话~自动放在页面底部
```

## 参考：

[nodeppt](https://github.com/ksky521/nodePPT/blob/master/README.md)