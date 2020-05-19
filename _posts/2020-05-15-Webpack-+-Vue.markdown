# 1.创建项目 webpack-vue
```
|- build
  |-- index.html
  |-- bundle.js(该文件是webpack打包后生成的)
|- app
  |-- components
      |--- Dialog.jsx
  |-- main.js
|- package.json(第二步生成)
|- webpack.config.js(第四步生成)
```
# 2.创建package.json  
```
npm init
```
# 3.安装webpack
```
//全局安装webpack，优点是打包时可以直接输webpack命令
npm install -g webpack
//在本项目中安装webpack，--save-dev的意思是将依赖写入项目的package.json文件
npm install --save-dev webpack
```

# 4.创建webpack.config.js配置文件
```
module.exports = { 
  entry: __dirname + "/app/main.js",//唯一入口文件，就像Java中的main方法 
  output: {//输出目录 
      path: __dirname + "/build",//打包后的js文件存放的地方 
      filename: "bundle.js"//打包后的js文件名 
  }
};
```
运行`webpack`，webpack非全局安装需输入`node_modules/.bin/webpack`

![webpack执行成功](http://upload-images.jianshu.io/upload_images/1874069-06f40d3be8344671.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

看到以上提示说明打包成功了，可以看到build目录下的bundle.js里多了很多自动生成的代码，但预览index.html却什么都没有看到，这是因为还没有往页面中写入内容。
# 5.更方便地执行打包命令
执行类似于`node_modules/.bin/webpack`这样的命令其实是比较烦人且容易出错的，不过值得庆幸的是npm可以引导任务执行，对其进行配置后可以使用简单的npm start命令来代替这些繁琐的命令。在package.json中对npm的脚本部分进行相关设置即可，设置方法如下。
打开package.json，找到script代码块，更改为：
```
"scripts": { 
    "build": "webpack"
    或者
    "start":"webpack"
}
```
但是start是一个特殊的脚本名称，如果脚本名称不是start，则需要`npm run (script name)`，如`npm run build`，但是start可以直接执行`npm start`

# 6.使用Source Maps，使调试更容易

| devtool选项 | 配置结果 |
| ------------- |-------------|
|source-map |在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它会减慢打包文件的构建速度 | 
| cheap-module-source-map| 在一个单独的文件中生成一个不带列映射的map，不带列映射提高项目构建速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便 | 
| eval-source-map | 使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。不过在开发阶段这是一个非常好的选项，但是在生产阶段一定不要用这个选项 | 
|cheap-module-eval-source-map|这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射，和eval-source-map选项具有相似的缺点|

>正如上表所述，上述选项由上到下打包速度越来越快，不过同时也具有越来越多的负面作用，较快的构建速度的后果就是对打包后的文件的的执行有一定影响。

>在学习阶段以及在小到中性的项目上，eval-source-map是一个很好的选项，不过记得只在开发阶段使用它，继续上面的例子，进行如下配置

```
/webpack.config.js
module.exports = { 
    devtool: 'eval-source-map',//生成Source Maps,这里选择eval-source-map 
    entry: __dirname + '/app/main.js',//唯一入口文件,__dirname是node.js中的一个全局变量，它指向当前执行脚本所在的目录 
    output: {//输出目录 
        path: __dirname + '/build',//打包后的js文件存放的地方 
        filename: 'bundle.js'//打包后输出的js的文件名 
    }
};
```

# 7.安装vue
```
npm install vue
```
# 8.安装必要的插件
```
npm install --save-dev babel-core babel-loader babel-preset-es2015
npm install --save-dev webpack-dev-server
npm install --save-dev node-sass sass-loader css-loader style-loader
```
