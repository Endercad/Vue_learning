### 基本结构

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app"></div>
    <!-- <script src="https:////unpkg.com/vue@next"></script>--> <!--CDN引入--> 
    <script src="./js/vue.js"></script> <!--本地引入-->
    <script>
        Vue.createApp({
            template:'<h1>Hello World</h1>',
            data(){
                return{
                }
            },
            methods:{         
            }
        }).mount('#app')
    </script>
</body>
</html>
```

***在使用createApp时，传入了一个对象***

#### template属性：

表示Vue需要帮助我们渲染的模板信息，模板里的标签内容会替换掉挂载元素的innerHTML（也就是原本的内容）

**template其他写法**

1.

```vue
<script type="x-template"  id="aaa">
    <div>
        <h1>
            Hello World
        </h1>
    </div>
</script>
<script>
        Vue.createApp({
            template:'#aaa',	<!--内部执行document.querySelect('#aaa'),把其内容作为template属性的值-->
            data(){
                return{
                }
            },
            methods:{         
            }
        }).mount('#app')
</script>
```

2**浏览器不会渲染template标签里的内容** 

```vue
<template  id="aaa">
    <div>
        <h1>
            Hello World
        </h1>
    </div>
</template>
<script>
        Vue.createApp({
            template:'#aaa',	<!--内部执行document.querySelect('#aaa'),把其内容作为template属性的值-->
            data(){
                return{
                }
            },
            methods:{         
            }
        }).mount('#app')
</script>
```

#### data属性

**传入一个函数，该函数需要返回一个对象**

#### methods属性

**methods属性是一个对象，在这个对象中定义方法，这些方法可以绑定到template模板中**

**在这些方法中，可以用this关键字直接访问data中返回对象的属性**

***注意***

***methods中定义的函数不能使用箭头函数***
***箭头函数里this指向window，就不能用this获取到data中的数据。箭头函数中不绑定this，用到this的时候会向上层作用域里找***









