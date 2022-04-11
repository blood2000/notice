[TOC]



## webpack学习笔记

###  **安装**

```java
//全局安装
npm install -g webpack  //方便但会由于不同项目的版本导致bug
//安装到项目目录
npm install --save-dev webpack
```

###  **正式使用前的准备**

```javascript
//创建package.json文件
npm init
```

### **正式使用Webpack**

#### *1，指定入口文件*

  ```javascript
//全局安装
# {extry file}出填写入口文件的路径
# {destination for bundled file}处填写打包文件的存放路径
# 填写路径的时候不用添加{}
webpack {entry file} {destination for bundled file}
//webpack非全局安装的情况
node_modules/.bin/webpack app/main.js public/bundle.js
  ```

#### *2，通过配置使用Webpack*

```javascript
//新建一个webpack.config.js文件
module.exports = {
  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
  output: {
    path: __dirname + "/public",//打包后的文件存放的地方
    filename: "bundle.js"//打包后输出文件的文件名
  }
}
```

```javascript
注：“__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录。
```

#### *3，更快捷的执行打包任务*

在命令行中输入命令需要代码类似于`node_modules/.bin/webpack`这样的路径其实是比较烦人的，不过值得庆幸的是`npm`可以引导任务执行，对`npm`进行配置后可以在命令行中使用简单的`npm start`命令来替代上面略微繁琐的命令。在`package.json`中对`scripts`对象进行相关设置即可，设置方法如下：

```json
{
  "name": "webpack-sample-project",
  "version": "1.0.0",
  "description": "Sample webpack project",
    
  "scripts": {
    "start": "webpack" // 修改的是这里，JSON文件不支持注释，引用时请清除
  },
    
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "3.10.0"
  }
}
```

```
//执行
npm start
```

#### ４，*生成Source Maps（使调试更容易）*

开发总是离不开调试，方便的调试能极大的提高开发效率，不过有时候通过打包后的文件，你是不容易找到出错了的地方，对应的你写的代码的位置的，`Source Maps`就是来帮我们解决这个问题的。

通过简单的配置，`webpack`就可以在打包时为我们生成的`source maps`，这为我们提供了一种对应编译文件和源文件的方法，使得编译后的代码可读性更高，也更容易调试。

在`webpack`的配置文件中配置`source maps`，需要配置`devtool`，它有以下四种不同的配置选项，各具优缺点，描述如下：

| devtool选项                    | 配置结果                                                     |
| :----------------------------- | :----------------------------------------------------------- |
| `source-map`                   | 在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的`source map`，但是它会减慢打包速度； |
| `cheap-module-source-map`      | 在一个单独的文件中生成一个不带列映射的`map`，不带列映射提高了打包速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便； |
| `eval-source-map`              | 使用`eval`打包源文件模块，在同一个文件中生成干净的完整的`source map`。这个选项可以在不影响构建速度的前提下生成完整的`sourcemap`，但是对打包后输出的JS文件的执行具有性能和安全的隐患。在开发阶段这是一个非常好的选项，在生产阶段则一定不要启用这个选项； |
| `cheap-module-eval-source-map` | 这是在打包文件时最快的生成`source map`的方法，生成的`Source Map` 会和打包后的`JavaScript`文件同行显示，没有列映射，和`eval-source-map`选项具有相似的缺点； |

对小到中型的项目中，`eval-source-map`是一个很好的选项，再次强调你只应该开发阶段使用它，我们继续对上文新建的`webpack.config.js`，进行如下配置:
