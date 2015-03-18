title: "mac 搭建hexo博客"
date: 2015-03-18 17:09:30
tags: 环境搭建
---
之前在ubuntu上搭建hexo博客感觉很简单，在网上找的安装教程，很容易就搭建好了。最近在mac上搭建hexo一直报error，但不影响使用。
对于追求完美的我来说，是受不了的。查了好多资料，一直没解决，今天突然想到是否是node版本的问题，果然是这样的。
下面，将mac搭建hexo过程及走过的一些坑记录下来：

###利用nvm安装nodejs

####安装nvm
如果没有安装curl，请先执行下面的指令：
```
brew install curl
```
安装nvm：
```
$ curl https://raw.github.com/creationix/nvm/master/install.sh | sh
```
####安装nodejs
```
$ nvm install 0.12
```
###安装hexo
```
$ npm install -g hexo-cli
```
###初始化博客
```
$ hexo init blog
```
```
$ cd blog
```
安装所需依赖：
```
$ npm install
```
至此，hexo博客环境已搭建完成。
###配置hexo
在根目录下，_config.yml文件是整个hexo的配置文件，首先，建立与github的链接

```
deploy:
  type: git
  repository: git@github.com:fgcui1204/fgcui1204.github.io.git
  branch: master
```
之前，type是github，现在的hexo版本好像改成git了，如果在此遇到这样的错误：
```
ERROR Deployer not found: git
```
需要执行：
```
npm install hexo-deployer-git --save
```
才可以正常的使用git。
###hexo基本使用
  hexo的基本使用在hexo自动生成的`hello world`页面有详细介绍，这里不再多说
  ，也可以通过`hexo --help` 来获得更多的帮助。


参考：


[hexo官网](http://hexo.io/)


[ERROR Deployer not found: github](https://github.com/hexojs/hexo/issues/1040)
