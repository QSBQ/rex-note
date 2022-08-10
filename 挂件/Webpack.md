# webpack

webpack是一个打包工具

webpack会让前端的所有资源文件(js/json/css/img/less/...)都作为模块处理 

作用：提高性能和提高兼容性

## webpack五个核心概念

**Entry**

入口指示以哪个文件为起点开始打包，分析构建内部依赖图

**Output**

输出指示打包后的资源 bundles 输出到哪里，以及如何命名

**Loader**

loader就是将webpack不认识的内容转化为认识的内容(webpack自身只认识JavaScript) 

**Plugins**

插件(Plugins)可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩， 一直到重新定义环境中的变量等

**Mode**

模式指示使用相应模式的配置

development==>本地调试运行环境 

production==>优化上线运行环境 

## webpack的初体验

- 初始化package.json-终端:

```
npm init -y
```

- 安装webpack-终端:

```
npm i webpack webpack-cli -g//教材版本是4.41.6  3.3.11
```

开发环境打包-终端：

```
webpack ./src/index.js -o ./dist/main.js --mode=development
```

功能：编译打包 js 和 json

生产环境打包-终端：

```
webpack ./src/index.js -o ./build --mode=production 
```

功能：编译打包 js 和 json，压缩代码

总结：webpack 能够编译打包 js 和 json，能压缩代码， 但不能编译打包 css、img……

**使用**

- 新建src文件夹和build文件夹
- src/index.js:

```
function add(x, y) {
    return x + y;
}
console.log(add(1, 2));
```

- 使用打包命令打包

## 打包webpack无法编译的资源

### 打包样式资源

- 下载-终端：

```
npm i css-loader style-loader -d
```

- webpack.config.js:

```
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'main.js',
        path: path.join(__dirname, 'build')
    },
    module: {
        rules: [{
            test: /\.css$/,
            use: [
                'style-loader',
                'css-loader'
            ]
        }]
    },
    plugins: [],
    mode: 'development'
}
```

注1：webpack.config.js是webpack的配置文件，用于指示webpack要做的事情

注2：所有构建工具都是基于node.js运行的,模块化默认采用commonjs

### 打包html资源

# webpack书

- 安装webpack-终端:

```
npm i webpack webpack-cli -d
```

- webpack.config.js:

```
const path = require('path');

module.exports = {
    mode: 'development', //模式
    entry: './src/index.js', //打包入口地址
    output: {
        filename: 'main.js', //输出文件名
        path: path.join(__dirname, 'dist') //输出文件目录
    }
}
```

打包命令-终端:

```
npx webpack
```

### 打包css

- 安装编译-终端:

```
npm i css-loader style-loader -d
```

- webpack.config.js:

```
const path = require('path');

module.exports = {
    mode: 'development', //模式
    entry: './src/index.js', //打包入口地址
    output: {
        filename: 'main.js', //输出文件名
        path: path.join(__dirname, 'dist') //输出文件目录
    },
    module: {
        rules: [ //转换规则
            {
                test: /\.css$/, //匹配所有的css文件
                use: ['style-loader', 'css-loader']
            }
        ]
    }
}
```

### 打包html