## 6.Node.js

1. 基本使用
2. express 框架
3. webpack 技术（其他功能相同技术）
4. npm 使用

# Node.js 及相关技术

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，简单点说，就是运行在服务端的 Javascript。
Node.js 使用 npm 作为包管理器。

## 开始使用及 npm

### 开始

安装好 Node.js，可以通过 CMD 操作。

- 在控制台输入 node -v 查看版本号
- 输入 node 进入 nodejs 环境，输入 console.log('hello');可以在环境中执行 js 代码。
- 在 E 盘新建文件 Hello.js，编写 console.log('hello');，从控制台进入 E 盘，执行 node hello.js

### npm

NPM 是随 NodeJS 一起安装的包管理工具。

- 允许用户从 NPM 服务器下载别人编写的第三方包到本地使用。
- 允许用户从 NPM 服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到 NPM 服务器供别人使用。

1. 安装
   在控制台输入 npm -v 查看版本号
   更换 npm 库的源（以下是阿里的源）
   npm config set registry https://registry.npm.taobao.org - 本地安装
   Package 会被下载到当前文件夹./node_modules 下，只能在当前目录下使用。
   Npm install 包名 - 全局安装
   Package 会被下载到特定的系统目录下，安装的 package 允许在所有目录下使用。
   查看全局安装包 - 查看删除
   npm -g ls 查看所有全局安装的包
   npm uninstall 包名 卸载包
2. 升级
   `npm install npm –g`
   `npm install npm@latest -g`
   查看版本号会发现已经升级到最新版。

## 使用 Node.js

## 使用 express

### 引入

`npm install express --save`

```
// 引入代码
const express = require('express')
const app = express()
```

### 监听

常用的有
|use|all|get|post|
| ------ | ------ | ------ | ------ |
|监听所有路由，第二个参数可以是 express.Router()等|监听所有 https|监听 get|监听 post|

```
 // app.METHOD(PATH, HANDLER)
app.get('/', function (req, res) {
    res.send('Hello World!')
})
// 通过 static 路径 访问 public文件夹下的静态文件
app.use('/static', express.static('public'))

// 监听8000端口
app.listen(8000, () => console.log('listening on port 3000!'))
```

### 数据库集成
#### mysql
[mysql](https://github.com/mysqljs/mysql)
` npm install mysql`

```
var mysql      = require('mysql');
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'me',
  password : 'secret',
  database : 'my_db'
});

connection.connect();

app.get("/getname", function(req, res) {
  var selectSQL = "select linkname from `localtest`";
  connection.query(selectSQL, function(err, rows) {
    if (err) throw error;
    for (var i = 0; i < rows.length; i++) {
      arr[i] = rows[i].linkname;
    }
    console.log(arr);
    res.send(arr);
  });
  //如果res.send 放到query外面，会造成第一次获得的是初始定义的arr。这么说的话，connection.query也是异步的
});

connection.end();
```



<!-- webpack下使用控制台启动服务器时，要先编译进入磁盘，再运行，而使用node.js的配置启动时，因为直接通过内存构建，不经过磁盘，所以运行效率会更高。

1. 启动
建立时选择Node.js express App,配置中template为EJS模板
    - 在package.json中有一个script属性，配置的就是启动名与启动路径，可以使用`npm run 名称`来运行，如果是默认的start,可以省略run
    - 也可以在在启动路径中设置的js文件中修改，例如默认的www.js,可以配置端口等，然后运行
2. 重构
按照工作要求/习惯重构项目 -->
