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