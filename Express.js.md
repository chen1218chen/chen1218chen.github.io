# Express.js

----

[TOC]

---

## Express安装
1. 全局安装express，express作为命令被安装到了系统中
```
npm install -g express
```
2. 查看express版本
```
express -V
```
3. 使用express命令创建工程，并支持ejs
```
> express -e nodejsDemo
>  cd nodejs-demo && npm install//安装依赖
```
4. 模板项目建立成功，启动模板项目
```
node app.js
```
5. 通过node启动程序，每次代码修改都需要重新启动。 有一个工具supervisor，每次修改代码后会自动重启，会我们开发省很多的时间
```
npm install -g supervisor
```
6. 再次启动服务
```
supervisor app.js
```

## 设置静态文件目录
项目使用bower来管理各个插件包，所以在项目中需要将**bower_components**设置为静态资源的根目录。express默认的静态文件目录为public下，也可以设置多个静态文件目录。
```
//设定静态文件路径
app.use(express.static(path.join(__dirname, 'public')));
app.use('/bower_components', express.static(path.join(__dirname,'/bower_components')));
```
如果不使用**path.join()**方法则可以直接使用字符串相加
```
app.use('/bower_components', express.static(__dirname+'/bower_components'));
```
## 文件访问
1. 首先，require加载路由
```
var routes = require('./routes/index');
var users = require('./routes/users');
var table = require('./routes/table');
```
2. 设置访问URI
```
app.use('/', routes);
app.use('/index', routes);
app.use('/users', users);
app.use('/table', table);
```
3. 访问地址
```
http://localhost:3000
http://localhost:3000/index
http://localhost:3000/users
http://localhost:3000/table
```
4. routes路由文件中
```
router.get('/', function(req, res, next) {
  res.render('index', { title: 'aa' });
});

router.get('/', function(req, res, next) {
  res.render('users', { title: 'users' });
})

router.get('/', function(req, res, next) {
 res.render('table', { title: '部门111' });
})

```
上面第一块代码意思是，get请求根目录则调用views文件夹中的index模板，并且传入参数title为“aa”，这个title就可以在模板文件中直接使用。

## 设置html引擎
1. 先对app.js做如下修改，再将view中页面改为.html
```
var ejs = require('ejs');
再将app.set('view engine', 'ejs'); 变成 
app.engine('.html', ejs.__express);
app.set('view engine', 'html');
```
若不修改view中页面，当引擎改为**html**引擎时，则将routes路由中的.ejs渲染写全文件名，如：
```
router.get('/', function(req, res, next) {
  res.render('users.ejs', { title: 'users' });
})
```
若渲染文件原本就是.html文件则可以不用写后缀名。并且.html文件中依然可以写```<%=title%>```这种类似的语法
2. 静态路由
添加主页路由
```
app.get('/', function(req, res) {
   res.sendfile('./views/index.html');
});

//路径参数获取
app.get('/:name', function(req, res) {
    var fileName = req.params.name;
    //console.log('./views/'+fileName);
    res.sendFile(fileName);
});
```
3. 配置supervisor热部署NodeJS
```
//安装
npm install -g supervisor
```
配置，在WebStorm中选择要Run--Edit Configurations 

Working directory选择项目目录（supervisor会监控此目录内所有文件的改变自动重启）
![enter description here][1]

  [1]: ./images/1445320217363.png "1445320217363.png"