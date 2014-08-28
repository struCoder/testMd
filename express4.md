### 1.3    使用Express框架开发Nodejs应用 

Express是基于Nodejs上一款非常优秀的Web服务器应用开发框架，可以说，Express专为Nodejs量身定做，  
使用Express可以让你开发Web应用非常的高效，
同样使用Express我们可以很轻松的搭建基于REST的Web服务，  
我们将通过循序渐进的方法一步步的带领读者初步的学习和理解Expess框架，  
在学完本节之后我们就可以写出一个完整的Web应用了。  

###1.3.1 开发环境
自己目前是使用的`Mac 64 bit` 机器，当然Express框架支持Windows以及Linux，同样的命令以及同样的操作，   唯一的区别就是在`Mac`或者
`Linux`上键入对应的命令的时候有时需要加上`sudo`也就是以管理员的身份安装这些应用。

开发环境的软件版本

+ Mac 64 bit
+ nodejs v0.10.28
+ npm v1.4.9
+ MongoDB v2.4.3

每一个Web程序都应当要与数据库打交道，除非是静态网站，在我们目前的项目中，   我们使用MongoDB作为我们这个应用的数据库，读者可MongoDB的官网下载稳定版本的MongoDB，   因为笔者的环境是Mac,在Mac下启动MongoDB服务器方法为：  
到含有mongod文件的目录下执行 ：`./mongod --dbpath= ../../mydata`,   参数--dbpath是本机保存用户数据的目录路径，读者可以根据那你的情况而定  

在Windows下命令差不多：

```{bash}
F:/MongoDB/mongod.exe --dbpath F:/MongoDB/data/mydata
```

启动Mongo程序
```{bash}
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
```


###1.3.2 建立工程  

应为笔者是在`Mac`下建立对应的`Nodejs`工程的,路径为 ：`cd /zZpig/Document/myFirstWebApp/`  
读者可以依据自己的环境搭建自己的`Nodejs`工程目录。  


在工作目录中创建工程，`Express3.x`中创建项目工程命令如下  

```{bash}
cd /zZpig/Document/myFirstWebApp/    //进入目录
npm install express -g               //全局安装express包
express -e myFirstWebApp             //通过express包，在当前目录新建myFirstWebApp工程，并使用EJS模板引擎
```

生成的项目结构，如下：

```{bash}
package.json         // 项目配置文件
app.js               // 程序启动文件
public               // Web目录 
/public/javascripts  //Javascript目录
/public/images       //存放图片的目录
/public/stylesheets  //css样式目录
/public/stylesheets/style.css  
/routes             //路由根目录
/routes/index.js  
/routes/user.js  
/views              //视图目录
/views/index.ejs   
```


相关命令的解释

当我们最初安装`Express3.x`的时候所加上的`-g`也就表示了，我们是通过全局的方式安装Express的。这样的好处就是，
我们可以直接在命令行中直接键入相应的命令执行相应的操作，而不是到安装的目录下再键入命令。  
在我们建立一个`web`工程的时候 `-e`参数是表示在创建项目时默认的使用`ejs`这样的模板引擎。

之后在工作目录中键入`node app.js` 或者`npm start`就可以启动我们的app了  

但是在目前的Express4.x中以上的命令不管用了.为此，我们还需要安装:`npm install -g express-generator`  
通过这个我们就可以像上面一样，进行工程的创建,特别需要注意的是，最好加上`sudo`也就是`sudo npm install -g express-generator`
在我们通过全局安装`Express4.x`之后，  
创建工程和`Express3.x`创建工程一样的命令也是通过`express -e myFirstWebApp`,然后我们到
的项目工程目录却比以前的多出了一个`bin`文件夹，在`bin`文件夹下是一个`www`的文件，这也是目前程序的主入口
。我们启动也不需要到这个目录下，而是在根目录下键入命令`npm start`稍等片刻，  
我们就可以在`localhost:3000`中看到我们第一个基于Nodejs的web应用了。  

在nodejs中每次更改代码后都要先结束当前运行的Nodejs进程，之后再启动才能运行改进之后的程序，   但是这样是很不方便的，在开发阶段推荐使用supervisor这个第三方包。  

安装 supervisor
```{bash}
sudo npm install supervisor -g      //进行全局安装supervisor包
```

启动我们的项目： `supervisor www`这里需要注意的是启动项目的时候一定要从项目主入口文件进行启动，不然会报错  
更多的关于supervisor使用方法的帮助：键入命令：`supervisor -h`    

###1.3.3 目录结构说明

+ node_modules, 存放所有的项目依赖库。(每个项目管理自己的依赖，与Maven,Gradle等不同)  
+ package.json，项目依赖配置及开发者信息  
+ app.js，程序主要加载以及初始化文件  
+ public，静态文件(css,js,img)  
+ routes，路由文件(MVC中的C,controller)  
+ Views，页面文件(Ejs模板)  
+ bin/www项目的主入口文件

这里有一点需要说明的是，关于模板引擎，在这里我们使用Ejs,语法简单十分且方便控制  

###1.3.4 app.js 中的配置

> 增加描述 app.js是什么，干什么用的，为什么要写这个配置。

```{bash}
var express = require('express');  
var bodyParser = require('body-parser');  
var logger = require('morgan');  
var cookieParser = require('cookie-parser');  
var app = express();  
```  

如何使用Express4.x，我们可以通过app.use()使用第三方提供的中间件或者自己写的中间件  
```
app.use(logger('dev'));  
app.use(bodyParser.json());  
app.use(bodyParser.urlencoded({extended: true}));  
app.use(cookieParser());  
app.use(express.static(path.join(__dirname,'public')));    
```

**在这里需要说明的是，我们在安装所需要的第三方包时，正确地方法如为在根目录键入命令：`npm install 所安装的包 --save`**
这样也就保证的当前所需安装包得版本号都背自动写入package.json这样的好处是方便以后工程的维护。  
关于`app.set()`的几点介绍：

```{bash}
app.set('port', process.env.PORT || 3000)：设置端口为 process.env.PORT 或 3000。
app.set('views', __dirname + '/views')：设置 views 
```

文件夹为存放视图文件的目录，即存放模板文件的地方，__dirname 为全局变量，存储当前正在执行的脚本所在的目录。
当我们设置我们项目究竟使用哪一个模板引擎时，
我们就通过  app.set('view engine', 'ejs')  将模板引擎设置为ejs。这里也就是说，不管你的项目里安装了几个模板引擎，用到的只有通过这条代码才能执行


## 1.3.5 Ejs模板引擎的介绍

###什么是模板引擎：

模板引擎（Template Engine）是一个将页面模板和要显示的数据结合起来生成 HTML 页面的工具。
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

+ <% code %>：JavaScript 代码。比如<% new Date().getTime() %> 这行代码执行就和正常的javascript代码一样的效果
+ <%= code %>：显示替换过 HTML 特殊字符的内容。
+ <%- code %>：显示原始 HTML 内容。
在这里 <%= code %> 和 <%- code %> 的区别，当变量 code 为普通字符串时，两者没有区别。当 code 比如为`<h1>Nodejs</h1>` 这种字符串时，<%= code %> 会原样输出h1大小的Nodejs，而 <%- code %> 则会显示 H1 大的 Nodejs字符串。  
语法就是这么简单。  

##1.3.6 使用Bootstrap  

这里也就是你在Bootstrap官网中下载它的一个包，将里面的*min.css和 *min.js分别放到/public/stylesheets和public/javascripts中，这里要注意的是，  
必须要引入jquery.js也是放到public/javascripts中之后  
在相关的页面文件中引入这些。  

接下来，我们把index.html页面切分成3个部分：header.html, index.html, footer.html。

+ header.html, 为html页面的头部区域 
+ index.html, 为内容显示区域  
+ footer.html，为页面底部区域  


header.html
```{html}
<!DOCTYPE html>  
<html lang="en">  
<head>  
<meta charset="utf-8">  
<title><%=: title %></title>  
<!-- Bootstrap -->  
<link href="/stylesheets/bootstrap.min.css" rel="stylesheet" media="screen">  
<!-- <link href="css/bootstrap-responsive.min.css" rel="stylesheet" media="screen"> -->  
</head>  
<body screen_capture_injected="true">  
 ```  
 

index.html
```{html}
<% include header %>  
<h1><%= title %></h1>  
<p>Welcome to <%= title %></p>  
<% include footer %>  

```
footer.html
```{html}
<script src="/javascripts/jquery-1.9.1.min.js"></script>  
<script src="/javascripts/bootstrap.min.js"></script>  
</body>  
</html>
```
##1.3.7路由功能
路由规划是整个网站的骨架部分，因为它处于整个架构的枢纽位置，相当于各个接口之间一个汇总，   因此，当我我们构建一个web应用时候，
应当优先考虑路由的问题，我们现在的设计如下：  

+ 路径'/'，映射到index.html页面也就是这是我们的主页，可以直接访问  
+ 路径'/home'，映射到home.html页面,这里设计为用户登录后才能访问的界面  
+ 路径'/login'，映射到login.html页面,也就是用户登录界面了，成功登录后跳转到home.html  
+ 路径'/logout'，当用户只要访问这个，默认为直接退出并且跳转到index.html页面  

相信求知若渴的你早已创建好项目了，我们现在修改一些东西：  
现在打开我们的`app.js`将第`25`行和第`9`行删掉，
也就是`app.use('/users', users);`和`var users = require('./routes/users');`目前我们这里不需要
在我们的router目录下的index.js设置成如下的：  

```{javascript}
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res) {
  res.render('index', { 
  	title: '欢迎加入Node' 
  });
});

router.get('/login', function(req, res) {
	res.render('login',{
		title:'用户登录'
	})
});
router.post('/login', function(req, res) {
	var user = {
		username:'Noder',
		password:'nodeAdmin'
	}
	if (req.body.username === user.username && req.body.password === user.password) {
		res.redirect('/home');
	}
});
router.get('/logout', function(req, res) {
	res.redirect('/');
});
router.get('/home', function(req, res) {
	var user = {
		username:'Noder',
		password:'nodeAdmin'
	}
	res.render('home',{
		title:'主页',
		user:user
	})
});

module.exports = router;

```  

现在在我们的`views`目录下创建login.ejs文件  

```{html}
<% include header %>
    <form class="form-inline" role="form" method="post">
        <div class="form-group">
            <label class="sr-only" for="username">Email address</label>
            <input type="text" required class="form-control" id="userId" name="username" placeholder="Enter ID">
        </div>
         <div class="form-group">
            <label class="sr-only" for="password">Email address</label>
            <input type="password" required class="form-control" id="userpwd" name="password" placeholder="Password">
        </div>
        <button type="submit" class="btn btn-primary">登录</button>
    </form>
<% include footer %>
```

同样的在`views`目录下创建home.ejs  
```
<% include header %>
	<h1>Welcome <%= user.username %>, 欢迎登陆！！</h1>
	<a claa="btn" href="/logout">退出</a>
<% include footer %>
```  

好了到目前为止，整个简单的web雏形已经完成了,让我们一起在**根目录**键入命令运行吧：  
```{bash}
npm start
```
感觉打开浏览器，键入`localhost:3000`  
读者将看到欢迎字样的标题。  

##1.3.8 会话支持(session)  

会话是一种持久的网络协议，用于完成服务器和客户端之间的一些交互行为。会话是一个比连接粒度更大的概念， 一次会话可能包含多次连接，每次连接都被认为是会话的一次操作。在网络应用开发中，有必要实现会话以帮助用户交互。例如网上购物的场景，用户浏览了多个页面，购买了一些物品，这些请求在多次连接中完成。许多应用层网络协议都是由会话支持的，如 FTP、Telnet 等，而 HTTP 协议是无状态的，本身不支持会话，因此在没有额外手段的帮助下，前面场景中服务器不知道用户购买了什么。
为了在无状态的 HTTP 协议之上实现会话，Cookie 诞生了。Cookie 是一些存储在客户端的信息，每次连接的时候由浏览器向服务器递交，服务器也向浏览器发起存储 Cookie 的请求，依靠这样的手段服务器可以识别客户端。我们通常意义上的 HTTP 会话功能就是这样实现的。具体来说，浏览器首次向服务器发起请求时，服务器生成一个唯一标识符并发送给客户端浏览器，浏览器将这个唯一标识符存储在 Cookie 中，以后每次再发起请求，客户端浏览器都会向服务器传送这个唯一标识符，服务器通过这个唯一标识符来识别用户。 对于开发者来说，我们无须关心浏览器端的存储，需要关注的仅仅是如何通过这个唯一标识符来识别用户。很多服务端脚本语言都有会话功能，如 PHP，把每个唯一标识符存储到文件中。
——《Node.js开发指南》   

其实会话支持，通俗点说就是服务器要知道是不是同一个用户进行的本次请求，因为HTTP协议是无状态的，
通过session可维持用户会话状态。  


在nodejs中支持会话，通过mongodb把话信息存储在数据库中以避免丢失
我们需要安装这么几个第三方包：  
```{bash}
npm install express-session --save  
npm install connect-mongo --save  
```

`connect-mongo`的作用是：将所连接的会话信息存储在MongoDB中  
`express-session`是express关于session存储设置的一个第三方包，他们的具体使用如下：  


安装好后，我们在app.js文件的第二行加上: 
```(javascirpt)
var session = require('express-session');
var MongoStore = require('connect-mongo')(session);  
```


并且在`app.use(express.static(path.join(__dirname, 'public')));`后加上：  
```(javascript)
app.use(session({
    secret:setting.cookieSecret,
    key:'noder',
    cookie:{maxAge:3600000 * 24 * 30},
    store:new MongoStore({
        db:'nodWeb',
        collection:'session'
    })
}));
``` 
参数解释：
> 
secret: 用来第三方防止篡改 cookie  
key: 就是cookie的名字  
cookie中的maxAge就是设定次cookie的生存周期  
store: 其参数为MongoStore的实例，也就是说将会话信息存到mongodb中以免丢失  



编辑app.js文件
```{javascript}
var cookieParser = require('cookie-parser');
var session = require('express-session');
var MongoStore = require('connect-mongo')(session);`  
app.use(cookieParser('secret'));  

//.. 省略部分代码

app.use(express.static(path.join(__dirname, 'public')));

// 增加session控制
app.use(session({                       
    secret:'secret',         
    key:'noder',                         
    cookie:{maxAge:3600000 * 24 * 30},   
    store:new MongoStore({              
        db:'nodWeb',
        collection:'session'
    })
}));

//.. 省略部分代码
```

在这里想必大家注意到了在`app.use(cookieParser('secret'))`中的字符串:`secret`与`app.use(session({...}))`  
中的secret的值一样，这也就是`cookie-parser`包的作用：解析http头中的cookie

紧接着我们在`router`目录下的index.js中的

```{bash}
router.post('/login', function(req, res) {
	var user = {
		username:'Noder',
		password:'nodeAdmin'
	}
	if (req.body.username === user.username && req.body.password === user.password) {
		res.redirect('/home');
	} else {
		res.redirect('login')
	}
	}
});

router.get('/logout', function(req, res) {
	res.redirect('/');
});
```  
改为：
```
router.post('/login', function(req, res) {
	var user = {
		username:'Noder',
		password:'nodeAdmin'
	}
	if (req.body.username === user.username && req.body.password === user.password) {
		req.session.user = user   //将用户信息保存到session中
		res.redirect('/home');
	} else {
		res.redirect('login')
	}
	}
});

router.get('/logout', function(req, res) {
	res.session.user = null;          //清空session中的信息，用户登出
	res.redirect('/');
});
```
 

##1.3.8 页面通知的显示  

**这里需要说明的是页面同时需要依赖于1.3.7中的session。**  
我们在这里要安装一个中间件`connect-flash;`通过`npm install connect-flash --save`  
**我们为什么要使用这个包**  
通过使用`connect-flash`我们可以将错误消息或者提示信息友好的展示给用户，让用户知道现在的状态，  
比如：用户登入失败，我们可以通过`connect-flash`相关的方法提示给用户。等等

对照使用：  
```
app.set('view engine', 'ejs');
app.use(flash());
```

我们在`header.ejs`中`<body>`加入以下代码：  
```
<div id="dialog">
    <% if(error) { %>
      <div class="alert alert-danger alert-dismissable">
        <button type="button" class="close" data-dismiss="alert" aria-hidden="true">&times;</button>
        <strong>警告！</strong><%= error %>
      </div>
    <% } %>

    <% if(success) { %>
      <div class="alert alert-success alert-dismissable">
        <button type="button" class="close" data-dismiss="alert" aria-hidden="true">&times;</button>
        <strong>提示：</strong><%= success %>
      </div>
    <% } %>
  </div>
```  
并且在router目录下的index.js中将所有`router.get()`函数中的`title`值下加上  
```
error:req.flash('error').toString(),
success:req.flash('success').toString()
```  
比如：  
```
router.get('/', function(req, res) {
  res.render('index', { 
  	title: '欢迎加入Node',  
  	error:req.flash('error').toString(),  
        success:req.flash('success').toString()  
  });  
});
```  
**如何使用：**  
在客户端经行post时，我们在对应的`router.post()`方法下加上如果有错误  
```
req.flash('error','请键入正确的密码');
return res.redirect('/login');
```
如果一个post操作成功就如下:  
```
req.flash('succss','登录成功');
return res.redirect('/Home');
```  

范例：
```
router.post('/login', function(req, res) {
	var user = {
		username:'Noder',
		password:'nodeAdmin'
	}
	if (req.body.username === user.username && req.body.password === user.password) {
		req.flash('success','登录成功');
		res.redirect('/home');
	} else {
		req.flash('error','请键入正确的密码');
	        res.redirect('/login');
	}
});
```  

##1.3.9 页面权限的控制  
这也是本章节的最后一项内容了。 

所谓的页面控制通俗的说就是有权限的用访问相应权限的页面，没有权限只能访问不需要权限的页面。  
本节我们讲述如何进行页面控制。  
我们在`router.get('/home', function(req, res){})`的上方加入  
```
router.use(function(req, res, next) {
	if (!req.session.user) {
		req.flash('error', '请先登录');
		return res.redirect('/login')
	}
	next();
});
```  

在`router.get('/login', function(req, res){})`上方加上：  
```
router.use(function(req, res) {
	if (req.session.user) {
		req.flash('error', '您已经登录');
		return res.redirect('/home')
	}
});
```  

本章节到此就完成了。在本章节中，我们通过一个简单的例子了解到如通过Express框架高效的开发Nodejs的web应用，
在这里笔者也是抛砖引玉。



