# 文件更改自动编译

## 1.watch

- 在package.json文件中设置scripts```"build":"webpack --watch"```

## 2.webpack-dev-server

- ```npm install webpack-dev-server -D```
  
- 在package.json文件中设置scripts```"serve":"webpack serve```

- webpack-dev-server 对资源没有输出文件，而是保存在内存中，服务器访问更快

## 3.webpack-dev-server配置

### 1.contentBase(static)

- 指定静态资源的根目录，如果涉及到webpack未打包的非webpack资源，由contentBase为其指定根目录
  
- **新版本webpack contentBase改为了static**

webpack.config.js

```js
devServer:{
    static:{
        directory:path.join(__dirname,"public")
    }
}
```

- 在开发阶段这样设置，打包阶段还是要用CopyWebpackPlugin把所有资源打包

### 2.hot

- HMR(Hot Module Replacement):模块热替换：在添加、替换、删除模块时，无需刷新整个页面

- 开启HMR```devServer:{hot:true}```

- 对需要热替换的模块的引入

```js
import "./js/aaa.js"
if(module.hot){
    module.hot.accept("./js/aaa.js")
}
```

- 在Vue组件中更改，Vue-loader默认已经实现模块热替换，不需要手动设置

### 3.proxy

  解决开发环境的跨域问题

#### 跨域

- 跨域的原因出于浏览器的同源策略，同源策略是浏览器最核心的安全功能

- 当一个请求的url的**协议、域名、端口**三者之间任意一个与当前页面url不同即为跨域

#### proxy原理

- 利用 http-proxy-middleware这个http代理中间件，实现请求转发给其他服务器,也就是请求变成了从一台服务器到另一台服务器，就避免了因浏览器同源策略产生的跨域问题

- 设置

```js
devServer:{
    proxy:{
        "/api":{
            target:"http://localhost:3000",
            pathRewrite:{"^/api":""},   //忽略api前缀
            //到/api/xxx的请求会被代理到http://localhost:3000/xxx
            secure:false,   //忽略https安全提示
            changeOrigini:true  //发送请求头中host会设置成target

        }
    }
}
```
