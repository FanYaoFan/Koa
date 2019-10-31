# Koa
## 目录  
## [1 koa](#1-koa)
## [2 GET请求](#2-get-请求)
## [3 POST请求](#3-post请求)
## [4 koa-router](#4-koa-router-1)
## [5 cookie](#5-cookie)
## [6 koa2模板](#6-koa2模板)
## [7 koa-static 静态资源中间件](#7-koa-static-静态资源中间件)
## [8 Mongodb](https://github.com/FanYaoFan/Koa/tree/master/Mongodb)  

***
___
## 1 koa
### 1.1 安装  
` nvm install 7`  
` npm i koa`  
` node my-koa-app.js`   
同样可以 `yarn add i koa2` 
### 1.2 使用  
`const Koa = require('koa')`  
`const app = new Koa()`   
`app.use( async ( ctx ) => { ctx.body = 'hello koa2'})`  
 `app.listen( 3000, () => { console.log('server is running')})`  
## 2 GET 请求  
实际开发中,在我们做页面的时候,我们要得到一个商品的详细信息，需要传递GET形式的商品编号到后台，后台返回给我们数据.我们拿到数据进行渲染
### 2.1 query和querystring区别  
在koa2中get请求通过request接收,但是接收的方法有两种 
1. query 
返回的是格式化好的参数对象
2. querystring
返回的是请求字符串  
get请求的方式有两种,一种是从request中获得,一种是直接从上下文获得.如何获得?  
1 通过request.query和request.querystring  
2 上下文获得 ctx.query / ctx.query.string 
## 3 POST请求 
原生node.js需要通过解析上下文context请求对象req来获取
步骤: 
1. 解析上下文ctx中的原生node.js对象req
2. 将post表单数据解析成query string 字符串(eg : user=jack&age=18)
3. 将字符串转换成JSON格式
###  3.1 ctx.request he ctx.req
ctx.request  
是koa2中context经过封装的请求对象,它用起来更直观和简单
ctx.req   
是node.js中context提供的原生HTTP请求对象. 
### 3.2 koa-bodyparser中间件
#### 3.2.1
1. 安装 
`yarn add i --save koa-bodyparser`
2. 引入
`const bodyParser = require('koa-bodyparser')`  
3. 使用  
app.use(bodyParser())  
在代码中使用,直接可以用ctx.request.body进行获取POST请求参数,中间件自动给我们做了解析
## 4 Koa-router  
### 4.1 介绍  
路由是一个有URI(路径)和一个特定的HTTP方法(GET/POST)组成的,设计到应用如何响应客户端对某个网站节点的访问.eg 登录页面/注册页面 根据请求url地址的不同,加载不同的页面实现不同的功能  
### 4.1.1
安装   
`yarn add i --save koa-router`  
引入   
`const Router = require ('koa-router')`    
使用   
`let router = new Router()`  
### 4.1.2 模块化  
实际项目中,跟前端类似,把业务路由模块化,一般在appAPI下  
如图 
* user.js 模块
<img src="https://github.com/FanYaoFan/Koa/blob/master/img/koa-routerModules1.png"></img>
注意:  `let router = new Router()`  
`router.use('/user',user.routes())`  
使用导入的路由模块
* index.js 界面
<img src="https://github.com/FanYaoFan/Koa/blob/master/img/koa-router-index.png"></img>
### 4.1.3 koa-cors 
安装 
`yarn add i koa-cors` 
引入
`const cors = require('koa-cors')`  
使用  
`app.use(cors())`   
koa-cors 是为了解决跨域问题  
## 5 cookie  
开发中制作登录和保存用户信息在本地,最常用的就是cookie操作  
读取上下文中请求的cookie  
`ctx.cookies.get(name,[options])`  
上下文写入cookie  
`ctx.cookies.set(name,value, [options])`  
### 5.1  cookie选项  
存储用户名,保留用户登录状态时,可以选择7天内不用登录,也可以选择30天内不用登录.   
* domain: 写入cookie所在的域名  
* path: 写入cookie所在的路径  
* maxAFW: cookie最大的有效时常  
* expires: cookie失效时间  
* httpOnly: 是否只用http请求中获得  
* overwrite:是否允许重写  
如图 
<img src="https://github.com/FanYaoFan/Koa/blob/master/img/cookie.png"></img>
## 6 Koa2模板  
### 6.1 ejs模板
安装 
` cnpm i --save koa-views`  
安装ejs模板引擎  
`npm i --save ejs`    
一般新建一个view的文件夹,并在它下面新建index.ejs文件  
`const views = require('koa-views')`  
`app.use(views(__dirname,, {extension: 'ejs'}))`  
### 6.2 art-template  
`yarn add i --save art-template`   
`yarn add i --save koa-art-template`  
`render( app,, {root : path.join(__dirnamek, 'view'), //视图的位置  `
`extname : '.art', 后缀名 })`  
## 7 koa-static 静态资源中间件  
安装 `yarn add i --save koa-static` 
<img src="https://github.com/FanYaoFan/Koa/blob/master/img/static.png"></img>
## 8 Mongodb 
[Mongodb](https://github.com/FanYaoFan/Koa/tree/master/Mongodb)  
