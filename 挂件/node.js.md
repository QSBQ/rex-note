# node.js

## 简介

- 浏览器是 JavaScript 的前端运行环境
- Node.js 是 JavaScript 的后端运行环境
- Node.js 中无法调用 DOM 和 BOM 等浏览器内置 API

## node.js的安装

1. 到官网安装node.js环境
2. 查看 Node.js 版本号:node –v 

使用node.js:

1. 打开终端，切换到文件所属目录
2. node 文件名

注:powershell终端是cmd终端的升级版

## 终端命令

① 使用 ↑ 键，可以快速定位到上一次执行的命令

② 使用 tab 键，能够快速补全路径

③ 使用 esc 键，能够快速清空当前已输入的命令

④ 输入 cls ，可以清空终端

## fs文件系统模块

fs 模块是用来操作文件的模块, Node.js 官方提供

fs.readFile(路径，[编码格式]，回调函数)==>用来读取文件内容

fs.writeFile(路径，写入内容，[编码格式]，回调函数)==>用来写入文件内容

- 读取文件-js:

```
const fs = require('fs');
fs.readFile('./knife.md', 'utf-8', function(err, dataStr) {
    console.log(err);
    console.log(dataStr);
})
```

- 写入文件-js:

```
fs.writeFile('resource/sword.md', '齐疯子', function(err) {
    if (err) {
        return console.log(`文件写入失败:${err.message}`)
    }
    console.log(`文件写入成功`)
})
```

## path路径模块

path模块是用来处理路径的模块

- path.join() 方法，用来将多个路径片段拼接成一个完整的路径字符串
- path.basename() 方法，获取路径中的最后一部分，经常通过这个方法获取路径中的文件名
- path.extname() 方法，获取路径中的扩展名部分

- 使用path.join()-js:

```
const path = require('path');
……
fs.readFile(path.join(__dirname, 'resource/knife.md'), 'utf-8', function(err, dataStr) {
    ……
})
```

- 使用path.basename()-js:

```
const path = require('path');
const knife = '/a/k.html';
const sword = path.basename(knife);
```

- 使用path.extname()-js:

```
const sword = path.extname(knife);
```

## http服务器模块

http 模块是创建 web 服务器的模块

通过 http 模块提供的 http.createServer() 方法，就能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务

http创建服务器-js:

```
const http = require('http');
const king = http.createServer();
king.on('request', function(req, res) {
    console.log('我是king');
});
king.listen(7534, function() {
    console.log('启动服务器之我是king');
})
```

req请求对象

只要服务器接收到了客户端的请求，就会调用通过 server.on() 为服务器绑定的 request 事件处理函数

js:

```
……
knife.on('request', (req, res) => {
    const ac = `${req.url}---${req.method}`;
    res.setHeader('Content-Type', 'text/html;charset=utf-8');
    res.end(ac);
})
……
```

根据不同地址响应不同内容-js:

```
……
const king = http.createServer();
king.on('request', (req, res) => {
    const knife = req.url;
    let sword = '';
    if (knife === '/') {
        sword = path.join(__dirname, '……');
    } else {
        sword = path.join(__dirname, '……', knife);
    }
    fs.readFile(sword, 'utf-8', function(err, dataStr) {
        ……
        if (err) return res.end('读取失败!!!');
        res.end(dataStr);
    })
})
……
```

#### 普通电脑和服务器的区别

------

注:消费资源的电脑，叫做客户端；提供资源的电脑，叫做服务器

服务器和普通电脑的区别在于，服务器上安装了 web 服务器软件，例如：IIS、Apache 等。通过安装这些服务器软件，就能把一台普通的电脑变成一台 web 服务器。

在 Node.js 中，我们不需要使用 IIS、Apache 等这些第三方 web 服务器软件。因为我们可以基于 Node.js 提供的

http 模块，通过几行简单的代码，就能轻松的手写一个服务器软件，从而对外提供 web 服务。

------

#### 服务器相关的概念

------

**IP 地址**

IP 地址就是互联网上每台计算机的唯一地址，因此 IP 地址具有唯一性。如果把“个人电脑”比作“一台电话”，那么“IP地址”就相当于“电话号码”，只有在知道对方 IP 地址的前提下，才能与对应的电脑之间进行数据通信。

IP 地址的格式：通常用“点分十进制”表示成（a.b.c.d）的形式，其中，a,b,c,d 都是 0~255 之间的十进制整数。例如：用点分十进表示的 IP地址（192.168.1.1）

注意：

① 互联网中每台 Web 服务器，都有自己的 IP 地址，可以在 Windows 的终端中运行 ping www.baidu.com 命令，即可查看到百度服务器的 IP 地址

② 在开发期间，自己的电脑既是一台服务器，也是一个客户端，为了方便测试，可以在自己的浏览器中输入 127.0.0.1 这个IP 地址，就能把自己的电脑当做一台服务器进行访问了。

**域名和域名服务器**

尽管 IP 地址能够唯一地标记网络上的计算机，但IP地址是一长串数字，不直观，而且不便于记忆，于是人们又发明了另一套字符型的地址方案，即所谓的域名（Domain Name）地址。

IP地址和域名是一一对应的关系，这份对应关系存放在一种叫做域名服务器(DNS，Domain name server)的电脑中。使用者只需通过好记的域名访问对应的服务器即可，对应的转换工作由域名服务器实现。因此，域名服务器就是提供 IP 地址和域名之间转换服务的服务器。

注意：

① 单纯使用 IP 地址，互联网中的电脑也能够正常工作。但是有了域名的加持，能让互联网的世界变得更加方便。

② 在开发测试期间， 127.0.0.1 对应的域名是 localhost，它们都代表我们自己的这台电脑，在使用效果上没有任何区别。

**端口号**

计算机中的端口号，就好像是现实生活中的门牌号一样。通过门牌号，外卖员可以在整栋大楼众多的房间中，准确把外卖送到你的手中。同样的道理，在一台电脑中，可以运行成百上千个 web 服务。每个 web 服务都对应一个唯一的端口号。客户端发送过来的网络请求，通过端口号，可以被准确地交给对应的 web 服务进行处理。

注意：

① 一个端口号不能同时被多个 web 服务占用

② 在实际应用中，URL 中的 80 端口可以被省略

------

## 模块化

解决一个复杂问题时，自顶向下逐层把系统划分成若干模块的过程

- king.js:

```
module.exports.name = 'Crazy';
```

- index.js:

```
const knife = require('./king');
console.log(knife.name);
```

#### 编程领域中的模块化

------

遵守固定的规则，把一个大文件拆成独立并互相依赖的多个小模块

代码模块化的好处：1.提高了代码的复用性;2.提高了代码的可维护性;3.可以实现按需加载

Node.js 中根据模块来源的不同，将模块分为3类：

- 内置模块(Node.js官方内置)
- 自定义模块(用户创建的 .js 文件)
- 第三方模块(第三方开发的模块，使用前需要下载)

使用require() 方法，可以加载需要的内置模块、用户自定义模块、第三方模块进行使用

注：使用 require() 方法加载其它模块时，会执行被加载模块中的代码

**module对象**

在每个 .js 自定义模块中都有一个 module 对象，它里面存储了和当前模块有关的信息

**module.exports对象**

在自定义模块中，可以使用 module.exports 对象，将模块内的成员共享出去，供外界使用。

外界用 require() 方法导入自定义模块时，得到的就是 module.exports 所指向的对象

注：使用 require() 方法导入模块时，导入的结果，永远以 module.exports 指向的对象为准

**exports 对象**

由于 module.exports 单词写起来比较复杂，为了简化向外共享成员的代码，Node 提供了 exports 对象。默认情况下，exports 和 module.exports 指向同一个对象。最终共享的结果，还是以 module.exports 指向的对象为准

------

## npm与包

Node.js 中的第三方模块又叫做包

- 安装包-终端:

```
 npm i moment
```

- 使用包-js:

```
const moment = require('moment');
const knife = moment().format('YYYY-MM-DD HH:mm:ss');
console.log(knife);
```

- 安装指定版本的包的命令-终端:

```
npm i moment@2.22.2
```

- 终端执行npm -v命令，可查看安装的 npm 包管理工具的版本号

- 创建 package.json-终端:

```
npm init -y
```

注：项目文件夹的名称一定要使用英文命名&&不能出现空格

**dependencies节点**

package.json 文件中，有一个 dependencies 节点，专门用来记录使用 npm i命令安装了哪些包

- 一次性安装所有的包-终端:

```
npm i
```

卸载指定的包-终端:

```
npm uninstall moment
```

**devDependencies节点**

仅在开发时用到的包记录到 devDependencies 节点中，开发和上线都要用到的包记录到 dependencies 节点中

- 下载时放置devDependencies-终端:

```
npm i moment -D
```

#### 包的基本知识

------

包的来源

不同于 Node.js 中的内置模块与自定义模块，包是由第三方个人或团队开发出来的，免费供所有人使用

-  https://www.npmjs.com/ 网站上搜索自己所需要的包
-  https://registry.npmjs.org/ 服务器上下载自己需要的包

npm包管理工具

可以使用npm包管理工具，从 https://registry.npmjs.org/ 服务器把需要的包下载到本地使用。

这个包管理工具的名字叫做 Node Package Manager（简称 npm 包管理工具），这个包管理工具随着 Node.js 的安装包一起被安装到了用户的电脑上。

初次装包后多出的文件

node_modules文件夹和package-lock.json配置文件

- node_modules 文件夹用来存放所有已安装到项目中的包。require() 导入第三方包时，就是从这个目录中查找并加载包
- package-lock.json 配置文件用来记录 node_modules 目录下的每一个包的下载信息，例如包的名字、版本号、下载地址等

包的语义化版本规范

包的版本号是以“点分十进制”形式进行定义的，总共有三位数字，如2.24.0

每一位数字所代表的的含义：

第1位数字：大版本

第2位数字：功能版本

第3位数字：Bug修复版本

版本号提升的规则：只要前面的版本号增长了，则后面的版本号归零

------

#### 包管理配置文件

------

npm 规定，在项目根目录中，必须提供一个叫做 package.json 的包管理配置文件。用来记录与项目有关的一些配置信息

如：

- 项目的名称、版本号、描述等
- 项目中都用到了哪些包
- 哪些包只在开发期间会用到
- 那些包在开发和部署时都需要用到

多人协作问题解决方案：共享时剔除node_modules

在项目中记录安装了哪些包

在项目根目录中，创建一个叫做 package.json 的配置文件，即可用来记录项目中安装了哪些包。从而方便剔除

node_modules 目录之后，在团队成员之间共享项目的源代码。

注：要把 node_modules 文件夹，添加到 .gitignore 忽略文件中。

运行npm init -y命令即可配置包管理配置文件

------

注:镜像（Mirroring）是一种文件存储形式，一个磁盘上的数据在另一个磁盘上存在一个完全相同的副本即为镜像

切换npm 的下包镜像源

下包的镜像源，指的就是下包的服务器地址

- 终端:

```
npm config get registry==>查看当前的下包镜像源
npm config set registry=https://registry.npm.taobao.org/==>下载并将下包的镜像源切换为淘宝镜像源
npm config set registry=https://registry.npmjs.org/==>切换当前镜像源为npm镜像源
```

包的分类：

使用 npm 包管理工具下载的包，分为项目包和全局包两大类

项目包

那些被安装到项目的 node_modules 目录中的包，都是项目包

项目包又分为两类，分别是：

- 开发依赖包（被记录到 devDependencies 节点中的包，只在开发期间会用到）
- 核心依赖包（被记录到 dependencies 节点中的包，在开发期间和项目上线之后都会用到）

全局包

在执行 npm install 命令时，如果提供了 -g 参数，则会把包安装为全局包

全局包会被安装到 C:\Users\用户目录\AppData\Roaming\npm\node_modules 目录下

注1：只有工具性质的包，才有全局安装的必要性。因为它们提供了好用的终端命令

注2：判断某个包是否需要全局安装后才能使用，可以参考官方提供的使用说明即可

i5ting_toc

i5ting_toc 是一个可以把 md 文档转为 html 页面的小工具

- 安装-终端：

```
npm i i5ting_toc -g
```

- 使用-终端：

```
i5ting_toc -f ./test.md -o
```

规范的包结构

一个规范的包，它的组成结构，必须符合以下 3 点要求：

① 包必须以单独的目录而存在

② 包的顶级目录下要必须包含 package.json 这个包管理配置文件

③ package.json 中必须包含 name，version，main 这三个属性，分别代表包的名字、版本号、包的入口

#### 开发自己的包

- src/a1.js:

```
//格式化时间的方法
function knife(a) {
    ……
}
……
module.exports = {
    knife
}
```

- index.js:

```
//包的入口文件
const n1 = require('./src/a1')
module.exports = {
    ...n1
}
```

- package.json:

```
{
    "name": "kqcrazy",
    "version": "1.0.0",
    "main": "index.js",
    "description": "格式化时间功能",
    "keywords": ["knife"],
    "license": "ISC"
}
```

- README.md:

```
## 安装
​```
npm i kqcrazy
​```

## 导入
​```
const kqc = require('kqcrazy')
​```

## 格式化时间
​```
// 调用knife对时间进行格式化
const dtStr = kqc.knife(new Date())
console.log(dtStr)
​```

## 开源协议
ISC
```

包的使用说明文档：

安装方式,导入方式,功能,开源协议

#### 发布包

1. 注册npm账号
2. 终端：

```
npm config set registry=https://registry.npmjs.org/ //切换当前镜像源为npm的命令
npm login//登录npm账号的命令,然后依次输入用户名、密码、邮箱、验证码
npm publish//发布包的命令
```

删除已发布的包

- 终端:

```
npm unpublish kqcrazy --force
```

注：① npm unpublish 命令只能删除 72 小时以内发布的包；② npm unpublish 删除的包，在 24 小时内不允许重复发布

## 模块的加载机制

模块在第一次加载后会被缓存,不论是内置模块、用户自定义模块、还是第三方模块，它们都会优先从缓存中加载，从而提高模块的加载效率

**内置模块的加载机制**

内置模块加载优先级最高

**自定义模块的加载机制**

使用 require() 加载自定义模块时，必须指定以 ./ 或 ../ 开头的路径标识符。没有指定会当作内置模块或第三方模块进行加载。

使用 require() 导入自定义模块时，如果省略文件扩展名，则 Node.js 会按顺序分别尝试：① 确切的文件名;②.js;

③  .json;④.node;⑤ 都不行，则报错

**第三方模块的加载机制**

如果传递给 require() 的模块标识符不是一个内置模块，也没有以 ‘./’ 或 ‘../’ 开头，则 Node.js 会从当前模块的父目录开始，尝试从 /node_modules 文件夹中加载第三方模块。如果没有找到对应的第三方模块，则移动到再上一层父目录中，进行加载，直到文件系统的根目录。

**目录作为模块**

当把目录作为模块标识符，传递给 require() 进行加载的时候，有三种加载方式：

①查找package.json 文件的 main 属性，作为 require() 加载的入口

②如果第一步不行，则 Node.js 将会加载 index.js 

③ 如果以上两步都不行，则打印错误消息，报告模块的缺失：Error: Cannot find module 'xxx'

# Express

Express 是Node.js的框架

1. Express 的作用和 Node.js 内置的 http 模块类似，是专门用来创建 Web 服务器的
2. Express 的本质：就是一个 npm 上的第三方包，提供了快速创建 Web 服务器的便捷方法

Express是基于Node.js 提供的原生 http 模块进一步封装出来的， http模块也可以创建 Web 服务器，但使用Express能够极大的提高开发效率

**Express 能做什么**

对于前端程序员来说，最常见的两种服务器，分别是：

- Web 网站服务器：专门对外提供 Web 网页资源的服务器
- API 接口服务器：专门对外提供 API 接口的服务器

答案：使用 Express，可以高效率创建 Web 网站服务器和API 接口服务器

安装-终端:

```
npm i express@4.17.1
```

- express创建服务器-js:

```
const express = require('express');
const knife = express();
knife.get('/', (req, res) => {
    res.send('恕瑞玛，你的皇帝回来了！！！');
})
knife.listen(5003, () => {
    console.log('服务器启动');
})
```

- 对外提供资源-js:

```
knife.use('/test.md', express.static('./test.md'));
```

**nodemon**

nodemon可以监听项目文件的变动，自动重启项目

- 安装-终端:

```
npm i nodemon -g
```

- 使用-终端:

```
nodemon .\n17_对外提供静态资源.js
```

#### Express 路由

路由就是映射关系

Express 中，路由指的是客户端的请求与服务器处理函数之间的映射关系

Express 中的路由分 3 部分组成，按顺序是请求的类型、请求的 URL 地址、处理函数

**路由的匹配过程**

请求服务器，路由匹配，按照定义顺序匹配，请求类型&&请求的URL匹配成功，才会调用对应的 function 函数进行处理

**模块化路由**

路由直接挂载到 app 上不方便管理，最好将路由抽离为单独的模块

- 主js:

```
const knife = require('./n18_2路由模块');//导入路由模块
king.use(knife);//注册路由模块
```

- 路由js:

```
const express = require('express');
const knife = express.Router();//创建路由对象
knife.get('/', (req, res) => {res.send('恕瑞玛，你的皇帝回来了！！！')});//挂载路由
module.exports = knife;
```

#### Express中间件

中间件（Middleware ），中间处理环节

Express 中间件的调用流程

当一个请求到达 Express 的服务器之后，可以连续调用多个中间件，从而对这次请求进行预处理

Express 的中间件，本质上是function处理函数

next 函数是实现多个中间件连续调用的关键，它表示把流转关系转交给下一个中间件或路由

**全局生效中间件**

客户端发起的任何请求，到达服务器之后，都会触发的中间件，叫做全局生效中间件

- 全局生效中间件-js:

```
const sword2 = (req, res, next) => {
    let a = '白蜮学宫';
    req.n = a;
    next();
}
king.use(sword2);
```

**中间件的作用**

多个中间件之间，共享同一份req和res。基于这样的特性，我们可以在上游的中间件中，统一为 req 或 res 对象添加自定义的属性或方法，供下游的中间件或路由进行使用

- 全局中间件-js:

```
……
const sword2 = (req, res, next) => {
    let a = '白蜮学宫';
    req.n = a;
    next();
}
king.use(sword2);
king.get('/', (req, res) => {
    res.send('恕瑞玛，你的皇帝回来了！！！' + req.n)
});
……
```

可以定义多个全局中间件，服务器会按顺序依次调用

**局部生效中间件**

不使用app.use() 定义的中间件，叫做局部生效中间件

- 局部中间件-js:

```
……
const sword = (req, res, next) => {
    let a2 = '六大夫';
    req.n2 = a2;
    next();
}
……
king.get('/ac', sword, (req, res) => {
    res.send('蛇众部' + req.n2)
})
……
```

中间件3个注意事项：① 一定要在路由之前注册中间件；② 调用 next() 函数后不要再写额外代码；③ 连续调用的多个中间件共享 req 和 res 对象

**中间件的分类**

- ① 应用级别的中间件-js:

```
const sword2 = (req, res, next) => {
    let a = '白蜮学宫';
    req.n = a;
    next();
}
king.use(sword2);
```

- ② 路由级别的中间件-js:

```
const sword2 = express.Router();
const sword2 = (req, res, next) => {
    let a = '白蜮学宫';
    req.n = a;
    next();
}
king.use(sword2);
```

一个挂载到express.Router(),一个挂载到express(),除此之外没有任何区别

- ③ 错误级别的中间件-js:

```
const er1 = (err, req, res, next) => {
    res.send('错误：' + err.message)
}
king.use(er1);//错误发生时，将被它捕获
```

注：错误级别中间件，必须注册在所有路由之后，而除了错误级别中间件，其它中间件必须注册在所有路由之前

- ④ Express 内置的中间件:

express.static 快速托管静态资源的内置中间件，例如： HTML 文件、图片、CSS 样式等

express.json 解析请求体数据( JSON 格式)

express.urlencoded 解析请求体数据(URL-encoded格式)

- ⑤ 第三方的中间件

如body-parser 这个第三方中间件，可以解析请求体数据

1. 安装-终端:

   ```
   npm i body-parser
   ```

2. 使用-js:

   ```
   const parser = require('body-parser');
   king.use(parser.urlencoded({ extended: false }));
   king.get('/', (req, res) => {
       console.log(req.body);
       res.send('恕瑞玛，你的皇帝回来了！！！')
   });
   ```

## 前后端协作

- 后端-js:

```
const express = require('express');
const king = express();
const cors = require('cors');
king.use(cors());
king.get('/', function(req, res) {
    if (req.query.name === 'a') {
        res.send(`疯狂的王后，crazy queen`);
    } else if (req.query.name === 'btn2') {
        res.send(`疯狂的王，crazy king`);
    } else {
        res.send(`没有匹配上`);
    }
})
king.listen(5003, function() {
    console.log('服务器启动');
})
```

- 前端-html:

```
……
    <button id="v1">按钮</button>
    <script>
        let v1 = document.getElementById('v1');
        let king = new XMLHttpRequest();
        v1.onclick = function() {
            king.open('get', 'http://127.0.0.1:5003?name=a', true);
            king.send(null);
            king.onreadystatechange = function() {
                if (king.readyState == 4 && king.status == 200 || king.readyState == 4 && king.status == 0) {
                    console.log(king.responseText);
                }
            }
        }
    </script>
……
```

# 前后端身份认证

目前主流的 Web 开发模式有两种，分别是：① 服务端渲染;② 前后端分离

**服务端渲染的Web开发模式**

服务端渲染的概念：服务器发送给客户端的 HTML 页面，是在服务器通过字符串的拼接，动态生成的。因此，客户端不需要使用 Ajax 这样的技术额外请求页面的数据

服务端渲染的优缺点

优点：①前端耗时少;② 有利于SEO

缺点：①占用服务器端资源;②无法分工合作

**前后端分离的 Web 开发模式**

前后端分离的概念：前后端分离的开发模式，依赖于 Ajax。后端只负责提供 API 接口，前端使用 Ajax 调用接口

前后端分离的优缺点

优点：①开发体验好;②用户体验好;③减轻服务器端渲染压力

缺点：① 不利于SEO，但有解决方案

**身份认证**

服务端渲染和前后端分离各自身份认证方案：

① 服务端渲染使用Session 认证机制

② 前后端分离使用JWT 认证机制

**Session 认证机制**

http协议的无状态性：客户端的每次 HTTP 请求都是独立的，连续多个请求之间没有直接关系，服务器不会主动保留每次 HTTP 请求的状态

突破HTTP 无状态的限制:使用Cookie,实现身份认证

**Cookie**

Cookie就是存储在浏览器中的字符串

它由一个名称（Name）、一个值（Value）和其它几个用于控制 Cookie 有效期、安全性、使用范围的可选属性组成

不同域名下的 Cookie 各自独立，每当客户端发起请求时，会自动把当前域名下所有未过期的 Cookie 一同发送到服务器

Cookie的四大特性：① 自动发送;② 域名独立;③ 过期时限;④ 4KB 限制

**Session 认证机制**

Cookie实现身份认证的过程：

客户端第一次请求服务器的时候，服务器通过响应头的形式，向客户端发送一个身份认证的 Cookie，客户端会自动将 Cookie 保存在浏览器中。随后，当客户端浏览器每次请求服务器的时候，浏览器会自动将身份认证相关的 Cookie，通过请求头的形式发送给服务器，服务器即可验明客户端的身份

Cookie不具有安全性，因为它容易被伪造

提高身份认证的安全性:Session 认证机制的精髓就是使用“会员卡+刷卡认证”的方式

使用:

- 安装-js:

```
npm i express-session
```

- 配置中间件-js:

```
const express = require('express');
const knife = express();
const session = require('express-session');
const ac = session({
    secret: '齐瑯琊',
    resave: false,
    saveUninitialized: true
})
knife.use(ac);
```

# 自己的理解

```
前端用ajax向已经存在的后端接口发送数据，后端接口处理数据，然后返回给前端，前端拿到数据，构建界面
一个软件的安全性和鲁棒性指的是：一个软件安全指的是它不容易被入侵，而具有鲁棒性则指的是它在被入侵并且损害一部分之后，依然能够在一定程度内正常使用

后台管理系统的作用：在前端制作页面的时候，定义好空位，然后所有空位填充的内容都全部调用后台管理系统中的接口的数据，后台管理系统的接口的数据放在一个地方，只需要修改这个地方的数据，前端页面就会相应的改变。
我将一幅标注设计图上传到后台管理系统，管理员根据标注，修改标注对应的图片，前端页面上对应的图片就会发生变化，所谓的后台管理系统，就是将包含数据的接口集合起来，前端通过调用不同的接口获取不同的数据构建前端页面，管理员利用接口的功能，简易的对数据作出修改，前端页面随之改变，这就是后台管理系统存在的使命.
也就是说，利用html+css+js+vue+node+ajax就可以制作一个具备后台和前台的企业网站！！！
```

```
客户端和服务端：
手机电脑浏览器网站软件都是客户端，提供资源的服务器就是服务端
而电脑也可以作为提供资源的服务器来使用，所以电脑也可以是服务端
```

```
跨域请求
浏览器安全的基石是同源策略，同源即：协议、域名、端口相同
违背同源策略就是跨域请求
```

```
主技能：html,css,js,vue,微信小程序,node.js,MySQL
辅助技能：less,ES6,git
```

