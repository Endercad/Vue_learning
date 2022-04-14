# Vue-Router

## vue-router安装

```npm install vue-router@4```

## vue-router 使用步骤

### 1.创建路由的组件

例如创建Home.vue和About.vue作为两个路由的页面

### 2.配置路由映射

创建router/index.js,并将组件导入进来
```import Home from '../pages/Home.vue'```
```import About from '../pages/About.vue'```

创建routes数组，里面包含一个个对象，标识组件与路由的映射关系

```js
const routes=[
    {path:'/home',component:'Home'},
    {path:'/about',component:'About'}
]
```

使用createRouter创建路由对象，并传入routes和history模式

```js
import {createRouter,createWebHashHistory} from 'vue-router'

import Home from '../pages/Home.vue'
import About from '../pages/About.vue'

const routes=[
    {path:'/home',component:Home},
    {path:'/about',component:About}
]

const router=createRouter({
    routes, //相当于`routes:routes`
    history:createWebHashHistory()
})

export default router
```

在根实例中使用router

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

createApp(App).use(router).mount('#app')
```

### 3.使用路由

使用router-link和router-view组件

## 路由默认路径

使用重定向，将根路径重定向到/home的路径下，这样就能让默认路径跳到首页并渲染组件

```js
const routes=[
    {path:'/',redirect:'/home'},
    {path:'/home',component:Home},
    {path:'/about',component:About}
]
```

## router-link 的属性

### to

to是一个字符串，或一个对象

### replace

若添加replace属性，则点击router-link时，会吊桶router.replace()而不是router.push()

### active-class

设置激活a元素后的class，默认是router-link-active

### exact-active-class

链接精准激活时，应用于渲染的<a> 的class，默认是router-link-exact-active

## 路由懒加载

路由懒加载，即只有使用到一个组件的时候，才加载它。
通过在配置映射关系时，设置如下

```js
const routes=[
    {path:'/',redirect:'/home'},
    {path:'/home',component:()=>import('../pages/Home.vue')},
    {path:'/about',component:()=>import('../pages/About.vue')}
]
```

## route对象的其他属性

### name

### meta

包含自定义数据

## 动态路由匹配

在VueRouter中，我们可以在路径中使用一个动态字段来实现，称之为路径参数
```{path:'/user/:id',component:()=>import('../pages/User.vue')},```
在router-link中进行如下跳转：
```<router-link to='/user/123'>用户</router-link>```

### 组件中获取传递的参数

在组件的template中，直接通过$route.params获取值
在组件的optionsAPI中，通过this.$route.params获取值
在组件的setup中，
```import {useRoute} from 'vue-router'```

```js
setup(){
    const route=useRoute()
    console.log(route.params)
}
```

## 路由嵌套

当组件中也需要路由时，要进行路由的嵌套，在route对象中，设置children属性，内容为子路由数组

```js
{
    path:'/home',
    component:()=>import('../pages/Home.vue'),
    children:[
        {
            path:'message',
            component:()=>import('../pages/HomeMessage.vue')
        },
        {
            path:'setting',
            component:()=>import('../pages/HomeSetting.vue')
        }
    ]
},
```

## 编程式导航

通过this.$router.push(),this.$router.replace(),this.$router.go(),this.$router.back(),this.$router.forward()等方法实现页面跳转。

通过query大的方式传递参数

```js
jumpToUser(){
    this.$router.push({
        path:'/user',
        query:{name:'cyh',age:22}
    })
}
```

在组件中通过$route.query获取参数

## router-link slot

```<router-link>```在默认情况下会将其包裹在```<a>```元素中，即使使用```<v-slot>```也是如此。设置custom属性，可以去除这种行为

```<router-link>```通过一个作用域插槽暴露底层地址能力

href：解析后的 URL。将会作为一个 ```<a>``` 元素的 href 属性。如果什么都没提供，则它会包含 base。

route：解析后的规范化的地址。

navigate：触发导航的函数。 会在必要时自动阻止事件，和 router-link 一样。例如：ctrl 或者 cmd + 点击仍然会被 navigate 忽略。

isActive：如果需要应用 active class，则为 true。允许应用一个任意的 class。

isExactActive：如果需要应用 exact active class，则为 true。允许应用一个任意的 class。

## 导航守卫

### beforeEach

