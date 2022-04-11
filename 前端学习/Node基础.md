## Node学习

[toc]

### 全局包查看

```js
npm list -g --depth 0
```

### 查看模块安装路径

```js
npm prefix -g
```

### express脚手架生成

> 已安装了express以及express-generator

1、搭建项目，创建基本文件

```js
express 项目名称
```

2、进入项目，安装依赖

```js
cd 项目名称
npm install
```

完成搭建

3、目录结构

```
项目目录
├─node_modules
├─app.js
├─package-lock.json
├─package.json
├─wyp.txt
├─views
├─routes
|   ├─index.js
|   └users.js
├─public
|   ├─stylesheets
|   |      └style.css
|   ├─javascripts
|   ├─images
├─bin
|  └www
```

4、启动项目命令

```js
npm start
```

浏览器输入localhost:3000即可访问

### node解决跨域

```js
const http = require('http');
const app = express();
http.createServer(app).listen(8088);
const tenderRouter = require('./routes/tender');  //例子
app.use(express.static('./dist'));
app.all('*', function(req, res, next) {
	res.header("Access-Control-Allow-Credentials", true);
	res.header('Access-Control-Allow-Origin', '*');
	res.header('Access-Control-Allow-Headers', 'Content-Type');
	res.header('Access-Control-Allow-Methods', 'PUT, POST, GET, DELETE, OPTIONS');
	//next();
	if (req.method === 'OPTIONS') {
		res.send(200);
	} else {
		next();
	}
	
});
app.use('/***', tenderRouter);
```

### node核心模块-util

#### util.inherits(constructor, superConstructor)

> util.inherits(constructor, superConstructor)是一个实现对象间原型继承的函数

```js
var util = require('util')
function Base() {
	this.name = 'base';
	this.base = 1991;
	this.sayHello = function() {
		console.log('Hello' + this.name);
	}
}
Base.prototype.showName = function() {
	console.log(this.name);
}
function Sub() {
	this.name = 'sub';
}
util.inherits(Sub, Base);
var objBase = new Base();
objBase.sayHello(); //Hello base
objBase.showName(); //base
var objSub = new Sub();
objSub.showName();//sub
```



