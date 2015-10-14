title: "chrome 开发者工具调试技巧(MAC版)"
date: 2015-04-26 14:05:59
tags: 工具
categories: 调试
---
##chrome常用快捷键

##调试技巧

###快速切换文件
- 在chrome开发者工具中，按`cmd`+`p`，就能快速搜寻和打开你的项目文件。
###在源代码中搜索

- 在页面已经加载的文件中搜寻一个特定的字符串，快捷键是`Cmd` + `Opt` + `F`，这种搜寻方式还支持正则表达式哦

###快速跳转到指定行

- 在Sources标签中打开一个文件之后，按 `Cmd` + `O`，然后输入`:`和行号，DevTools就会允许你跳转到文件中的任意一行。

###在控制台选择元素

- DevTools控制台支持一些变量和函数来选择DOM元素

  ·$()–document.querySelector()的简写，返回第一个和css选择器匹配的元素。例如$(‘div’)返回这个页面中第一个div元素

  .$$()–document.querySelectorAll()的简写，返回一个和css选择器匹配的元素数组。

  .$0-$4–依次返回五个最近你在元素面板选择过的DOM元素的历史记录，$0是最新的记录，以此类推。

  想要了解更多控制台命令，戳这里：[Command Line API](https://developer.chrome.com/devtools/docs/commandline-api)

###使用多个插入符进行选择

- 当编辑一个文件的时候，你可以按住cmd，在你要编辑的地方点击鼠标，可以设置多个插入符，这样可以一次在多个地方编辑。

###优质打印

- DevTools有内建的美化代码，可以返回一段最小化且格式易读的代码。Pretty Print的按钮在Sources标签的左下角。

###设备模式

![devEquipment.jpg](/img/devEquipment.jpg)

###颜色选择器

- 当在样式编辑中选择了一个颜色属性时，你可以点击颜色预览，就会弹出一个颜色选择器。当选择器开启时，如果你停留在页面，鼠标指针会变成一个放大镜，让你去选择像素精度的颜色。
