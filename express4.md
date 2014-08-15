### 1.3    使用Express框架开发Nodejs应用

####目录
本章重点介绍Express4.x的开发框架，其中还会涉及到Mongoose,Ejs,Bootstrap等相关内容。

+ 开发环境
+ 建立工程
+ 目录结构
+ Express4.x配置文件
+ Ejs模板使用
+ Bootstrap界面框架
+ 路由功能
+ Session使用
+ 页面提示
+ 页面访问控制  

###1.3.1 开发环境
`Mac 64 bit`  
MongoDB v 2.4.3
>
Tue May 14 09:24:50.118 [initandlisten] MongoDB starting : pid=1716 port=27017 dbpath=./data 64-bit host=zZpig mac-mini  
Tue May 14 09:24:50.119 [initandlisten] db version v2.4.3  
Tue May 14 09:24:50.119 [initandlisten] git version: fe1743177a5ea03e91e0052fb5e2cb2945f6d95f  
Tue May 14 09:24:50.119 [initandlisten] build info: windows sys.getwindowsversion(major=6, minor=1, build=7601, platform=2,   service_pack='Service Pack 1') BOOST_LIB_VERSION=1_49  
Tue May 14 09:24:50.119 [initandlisten] allocator: system  
Tue May 14 09:24:50.119 [initandlisten] options: { dbpath: "./Mongodb/data" }  
Tue May 14 09:24:50.188 [initandlisten] journal dir=./data\journal  
Tue May 14 09:24:50.189 [initandlisten] recover : no journal files present, no recovery needed  
Tue May 14 09:24:50.441 [initandlisten] preallocateIsFaster=true 3.26  
Tue May 14 09:24:50.778 [initandlisten] preallocateIsFaster=true 5.88  
Tue May 14 09:24:51.827 [initandlisten] waiting for connections on port 27017  
Tue May 14 09:24:51.827 [websvr] admin web console waiting for connections on port 28017    

`nodejs v 0.10.28`
`npm v 1.4.9`  

###1.3.2 建立工程  
路径 ：`cd /zZpig/Document/worke/`    
创建工程，因为在express3.x中只需要在命令行中键入 `npm install express -g`之后就将express进行全局安装，这也就意味着你可以  
通过命令:`express -e myFirstWebApp`在你的工作目录中创建一个如以下的文件结构：  
>
package.json  
app.js  
public  
/public/javascripts  
/public/images  
/public/stylesheets  
/public/stylesheets/style.css  
/routes  
/routes/index.js  
/routes/user.js  
/views  
/views/index.ejs    

之后在工作目录中键入`node app` 或者`npm start`就可以启动我们的app了  
但是在目前的Express4.x中以上的命令不管用了.为此，我们还需要安装:`npm install -g express-generator`  
通过这个我们就可以像上面一样，进行工程的创建,特别需要注意的是，最好加上`sudo`也就是`sudo npm install -g express-generator`  
在nodejs中每次更改代码后都要先结束当前运行的Nodejs进程，之后再启动才能运行改进之后的程序，但是这样是很不方便的，推荐使用  
pm2这个第三方包。  
### 安装：`sudo npm install pm2@latest -g`  
启动我们的项目： `pm2 start app.js`  
更多的关于pm2使用方法的帮助：键入命令：`pm2 -h`    

###1.3.3 目录结构说明

node_modules, 存放所有的项目依赖库。(每个项目管理自己的依赖，与Maven,Gradle等不同)  
package.json，项目依赖配置及开发者信息  
app.js，程序启动文件  
public，静态文件(css,js,img)  
routes，路由文件(MVC中的C,controller)  
Views，页面文件(Ejs模板)  

这里有一点需要说明的是，关于模板引擎，在这里我们使用Ejs,其渲染速度比jade要快，而且语法简单十分方便控制  

###1.3.4 app.js 中的配置
>  
var express = require('express');  
var bodyParser = require('body-parser');  
var logger = require('morgan');  
var cookieParser = require('cookie-parser');  
var app = express();  

因为在Express4.x中去除了对中间件的依赖，所以我们得这样使用中间件：
>app.use(logger('dev'));  
app.use(bodyParser.json());  
app.use(bodyParser.urlencoded({extended: true}));  
app.use(cookieParser());  
app.use(express.static(path.join(__dirname,'public')));    
诸如此类

**在这里需要说明的是，我们在安装所需要的第三方包时，正确地方法如为在根目录键入命令：  
`npm install 所安装的包 --save`**
这样也就保证的当前所需安装包得版本号都被自动写入package.json这样的好处是方便以后工程的维护。  
关于`app.set()`的几点介绍：
>
app.set('port', process.env.PORT || 3000)：设置端口为 process.env.PORT 或 3000。
app.set('views', __dirname + '/views')：设置 views 文件夹为存放视图文件的目录，即存放模板文件的地方，__dirname 为全局变量，存储当前正在执行的脚本所在的目录。
app.set('view engine', 'ejs')：设置视图模版引擎为 ejs。


## 1.3.5 Ejs模板引擎的介绍
###什么是模板引擎：
>模板引擎（Template Engine）是一个将页面模板和要显示的数据结合起来生成 HTML 页面的工具。
如果说上面讲到的 express 中的路由控制方法相当于 MVC 中的控制器的话，那模板引擎就相当于 MVC 中的视图


###在我们的项目中使用模板引擎:
我们在上面已经说了，通过：
app.set('view engine', 'ejs');可以设置视图模版引擎为 ejs。
使用的前提就是你已经通过`npm install ejs --save`将ejs第三方包安装完成
`渲染视图`
>在 routes/index.js 中通过调用 res.render() 渲染模版，并将其产生的页面直接返回给客户端。  
它接受两个参数，第一个是模板的名称，即 views 目录下的模板文件名，扩展名 .ejs 可选。第二个参数是传递给模板的数据对象，用于模板翻译。  
也就是诸如`res.render('模板名称',{各种参数})`  

####Ejs语法：  
ejs 的标签系统非常简单，它只有以下三种标签：

+ <% code %>：JavaScript 代码。
+ <%= code %>：显示替换过 HTML 特殊字符的内容。
+ <%- code %>：显示原始 HTML 内容。
在这里 <%= code %> 和 <%- code %> 的区别，当变量 code 为普通字符串时，两者没有区别。当 code 比如为`<h1>Nodejs</h1>` 这种字符串时，<%= code %> 会原样输出h1大小的Nodejs，而 <%- code %> 则会显示 H1 大的 Nodejs字符串。  
语法就是这么简单。  

##1.3.6 使用Bootstrap  

这里也就是你在Bootstrap官网中下载它的一个包，将里面的*min.css和 *min.  js分别放到/public/stylesheets和public/javascripts中，这里要注意的是，必须要引入jquery.js也是放到public/javascripts中之后
在相关的页面文件中引入这些。  

接下来，我们把index.html页面切分成3个部分：header.html, index.html, footer.html

header.html, 为html页面的头部区域  
index.html, 为内容显示区域  
footer.html，为页面底部区域  


header.html
>
`<!DOCTYPE html>`  
`<html lang="en">`  
`<head>`  
`<meta charset="utf-8">`  
`<title><%=: title %></title>`  
`<!-- Bootstrap -->`  
`<link href="/stylesheets/bootstrap.min.css" rel="stylesheet" media="screen">`  
<!-- <link href="css/bootstrap-responsive.min.css" rel="stylesheet" media="screen"> -->  
`</head>`  
`<body screen_capture_injected="true"> `   
 

index.html
>
`<% include header.html %>`
`<h1><%= title %></h1>`
`<p>Welcome to <%= title %></p>`
`<% include footer.html %>`  


footer.html
>
`<script src="/javascripts/jquery-1.9.1.min.js"></script>`
`<script src="/javascripts/bootstrap.min.js"></script>`
`</body>`
`</html>`

##1.3.7路由功能


