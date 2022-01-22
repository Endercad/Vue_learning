# Webpack路径配置

webpack中的resolve配置如何寻找模块对应的文件

## alias

resolve.alias配置项通过别名把原导入路径映射成新的导入路径

```js
resolve:{
    alias:{
        "js":path.join(__dirname,"./src/js")
    }
}
```

## extensions

在导入语句没有带后缀时，webpack自动带上后缀去尝试访问文件是否存在

默认是```extensions:['.js','.json']```

手动配置```extensions:['.js','.json','vue','te']```就可以省略.vue .ts的后缀
