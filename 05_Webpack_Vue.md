# Webpack中的Vue代码打包

## 1.index.html模板配置

webpack.config.js

```js
    const HtmlWebpackPlugin = require('html-webpack-plugini')
    module.exports={
        plugins:[
            new HtmlWebpackPlugin({
                template:'./public/index.html',
                title:'title'   
                //在模板中有<title><%= htmlWebpackPlugin.options.title %></title>
            })
        ]
    }
```

./public/index.html

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>

    <div id="app"></div>
  </body>
</html>

```

BASE_URL要通过webpack自带插件DefinePlugin设置

BASE_URL其实就是项目根目录

```js
import {DefinePlugin}=require('webpack')
module.exports={
    plugins:[
        new DefinePlugin({
            BASE_URL:"'./'"
        })
    ]
}
```

## 2.如果要渲染template标签，要用vue源代码的runtime+complier版本

```import {createApp} from 'vue/dist/vue.esm-bundler'```

## 3.一般用sfc的方式（单文件组件）
  
- ```npm install vue-loader@next -D```加载.vue文件的loader

- ```npm install @vue/complier-sfc -D```真正解析要用这个

## 4.VueLoaderPlugin插件

- ```const {VueLoaderPlugin}=require('vue-loader/dist/index')```

- ```pluginis:[new VueLoaderPlugin()]```
  