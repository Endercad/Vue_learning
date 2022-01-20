# Webpack

## 1. Webpack介绍

webpack是一个静态的模块化打包工具，为现代的JavaScript应用程序

### 解释

1.打包

2.静态（static）：最终将代码打包成静态资源

3.模块化（module):webpack默认支持模块化开发

## 2.Webpack如何对项目打包

1.根据命令或者配置文件找到入口文件（默认为src文件夹下的index.js）

2.从入口开始，生成一个依赖关系图，包含应用程序中所需的所有模块

3.将图中的模块一个个打包

**注意**:只有在index.js中引入的模块才会被打包

## 3.loader

Webpack对.css等文件不能直接加载，需要制定对应的loader才能完成

加载css文件常用的loader是css-loader

在终端执行```npm install css-loader -D```

### 1.loader使用

1.在引入时使用：```import "css-loader!../css/style.css"```（！分割，表示使用css-loader）

2.配置方式：在webpack.config.js文件中写明配置信息

在module.rule中配置loader

#### webpack.config.js文件如下

```js
const path = require('path');

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "./build"),
    filename: "bundle.js"
  },
  module: {
    rules: [
      {
        test: /\.css$/, //正则表达式
        // 1.loader的写法(语法糖)
        // loader: "css-loader"

        // 2.完整的写法
        use: [
          // {loader: "css-loader"}
          "style-loader",
          "css-loader",
          "postcss-loader"
          // {
          //   loader: "postcss-loader",
          //   options: {
          //     postcssOptions: {
          //       plugins: [
          //         require("autoprefixer")
          //       ]
          //     }
          //   }
          // }
        ]
      },
      {
        test: /\.less$/,
        use: [
          "style-loader",
          "css-loader",
          "less-loader"
        ]
      },
      // {
      //   test: /\.(less|css)$/,
      //   use: [
      //     "style-loader",
      //     "css-loader",
      //     "less-loader"
      //   ]
      // }
    ]
  }
}
```

### 2.style-loader

要将解析的css插入页面中，还需要style-loader,use中执行顺序是自下往上，style-loader要写在css-loader的上面

### 3.加载资源

#### 在webpack5之前，加载资源需要使用url-loader、file-loader

#### 在webpack5，直接使用资源模块类型，替代url-loader、file-loader

webpack5使用四种新增的资源模块（Asset Modules）替代了这些loader的功能。

##### 1.asset/resource 将资源分割为单独的文件，并导出url，就是之前的 file-loader的功能

##### 2.asset/inline 将资源导出为dataURL（url(data:)）的形式，之前的 url-loader的功能

##### 3.asset/source 将资源导出为源码（source code）. 之前的 raw-loader 功能

##### 4.asset 自动选择导出为单独文件或者 dataURL形式（默认为8KB）. 之前有url-loader设置asset size limit 限制实现

##### 写法

```js
module.exports={
    mode:"development",
    module:{
        rules:[
            {
                test:/\.(jpe?g|png|svg)$/,
                type:"asset",
                generator:{
                    filename:"img/[name]_[hash:6][ext]",  //指定文件名
                },
                parser:{
                    dataUrlCondition:{
                        maxSize:100*1024   //转成base64的条件
                    }
                }

            }
        ]
    }
}
```
## Webpack-Plugins

Webpack plugin 是一个具有应用方法的JavaScript对象，应用方法在编译时被调用

### webpack plugin 配置
```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: 'babel-loader',
      },
    ],
  },
  plugins: [
    new webpack.ProgressPlugin(),
    new HtmlWebpackPlugin({ template: './src/index.html' }),
  ],
};
```