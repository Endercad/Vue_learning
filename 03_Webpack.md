# Webpack
## 1. Webpack介绍
webpack是一个静态的模块化打包工具，为现代的JavaScript应用程序<br/>
### 解释：<br/>
1.打包<br/>
2.静态（static）：最终将代码打包成静态资源<br/>
3.模块化（module):webpack默认支持模块化开发<br/>
## 2.Webpack如何对项目打包
1.根据命令或者配置文件找到入口文件（默认为src文件夹下的index.js）<br/>
2.从入口开始，生成一个依赖关系图，包含应用程序中所需的所有模块<br/>
3.将图中的模块一个个打包<br/>
**注意**:只有在index.js中引入的模块才会被打包
## 3.loader
Webpack对.css等文件不能直接加载，需要制定对应的loader才能完成<br/>
加载css文件常用的loader是css-loader<br/>
在终端执行```npm install css-loader -D```<br/>
### loader使用：
1.在引入时使用：```import "css-loader!../css/style.css"```（！分割，表示使用css-loader）

2.配置方式：在webpack.config.js文件中写明配置信息<br/>
在module.rule中配置loader<br/>
**webpack.config.js文件如下**
```
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

### 要将解析的css插入页面中，还需要style-loader,use中执行顺序是自下往上，style-loader要写在css-loader的上面

