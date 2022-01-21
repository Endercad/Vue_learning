# Babel

## 什么是babel

Babel是一个工具链，用于将ECMAScript 2015+的代码转换为向后兼容版本的Javascript，包括语法转换，源代码转换等

Babel可作为一个独立的工具，可以不和webpack配置，单独使用

在命令行中使用babel，安装如下库
1.@babel/core:babel的核心代码
2.@babel/cli:可以让我们在命令行中使用babel

## babel使用

转换每种语法，都要安装对应的插件

babel提供预设preset:@babel/preset-env

### 命令行下转换命令

指定文件名的转换```npx babel filename.js --out-file result.js --presets=@babel/preset-env```

指定文件夹的转换```npx babel filename.js --out-dir dir --pre sets=@babel/preset-env```

### webpack中配置

```js
    rules:[
        {
            test:"/\.js$/,
            use:{
                loader:"babel-loader",
                options:{
                    presets:[
                        "@babel/preset-env"
                    ]
                }
            }
        }
    ]
```

或者将options写在单独的babel.config.js文件中

babel.config.js

```js
    module.exports={
        presets:[
            "@babel/preset-env"
        ]
    }
```
