# Composition API

## Options API

- Options API的一大特点就是在对应的属性中编写对应的功能模块

- 比如data定义数据、methods中定义方法、computed中定义计算属性、watch中监听属性改变，也包括生命
周期钩子

### Options API 的弊端

- 当我们实现某一个功能时，这个功能对应的代码逻辑会被拆分到各个属性中

- 当我们组件变得更大、更复杂时，逻辑关注点的列表就会增长，那么同一个功能的逻辑就会被拆分的很分散

## Composition API 认识

- 为了使用Composition API ，在Vue组件里的setup函数中编写代码

- setup是组件的另一个选项，可以用它代理大部分其他选项如methods、computed、watch、data、生命周期等

## Composition API 使用

### setup函数

#### setup函数的参数

setup函数有两个参数：props、context。因此props选项在Vue中还需要写

##### props:父组件传递的属性

- 在setup中不能用this获得props，因此需要传递props参数

##### context:上下文

context包含三个属性

- attrs:所有的非prop的attribute

- slots:父组件传递过来的插槽

- emit:当我们组件内部需要发出事件时会用到emit（因为我们不能访问this，所以不可以通过this.$emit发出事件）

#### setup函数的返回值

- setup的返回值可以在模板template中被使用

```js
setup(props,context){

    var count=100

    const increment =()=>{
        count++
        console.log(count);
    }

    return{
        title:"Hello Home",
        count,
        increment
    }
}
```

这样，在template中就可以使用mustache语法{{title}}使用title

- 在setup中的变量改变时，页面中的变量不会更新。在data()中变量能够响应式是因为在data中返回对象时，会用reactive函数对其进行处理，返回一个响应式的对象，它会收集当前界面对数据的依赖，数据改变时，页面会进行刷新。

### Reactive API

- 要使setup中定义的数据具有响应式的特性，可以使用reactive函数

```js
const state=reactive({
    counter:100
})
```

错误用法```const counter=reactive(100)```

传入的参数应该是对象或者数组

- 当我们使用reactive函数处理我们的数据之后，数据再次被使用时就会进行依赖收集

- 当数据发生改变时，所有收集到的依赖都是进行对应的响应式操作（比如更新界面)

### Ref API

- 如果我们传入一个基本数据类型,可以使用ref API

- ref 会返回一个可变的响应式对象，该对象作为一个响应式的引用维护着它内部的值，这就是ref名称的来源

- 它内部的值是在ref的value 属性中被维护的,在template中使用ref返回的对象时，Vue会自动解包，直接显示它的value属性。在逻辑代码里面没有自动解包，需要对对象里的value属性操作

### Readonly API

- 某些情况下，我们传入给其他地方（组件）的这个响应式对象希望在另外一个地方（组件）被使用，但是不能被修改，Vue3为我们提供了readonly的方法

- preadonly会返回原生对象的只读代理（也就是它依然是一个Proxy，这是一个proxy的set方法被劫持，并且不能对其进行修改）

### toRefs API

- 如果我们使用ES6的解构语法，对reactive返回的对象进行解构获取值，那么之后无论是修改结构后的变量，还是修改reactive返回的state对象，数据都不再是响应式的——解构时相当于把对象里的值直接赋值给变量，变量和state对象之间也不存在联系，所以不是响应式的

- toRefs的函数，可以将reactive返回的对象中的属性都转成ref

```js
const info =reactive({name:'cyh',age:22})
let {name,age}=toRefs(info) //相当于{name:ref(info.name),age:ref(info.age)}
```

- 这种做法相当于已经在state.name和ref.value之间建立了链接，任何一个修改都会引起另外一个变化

### toRef API

- 对reactive对象中的一个属性进行转换成ref，建立连接

```let name=toRef(info,"age")```

### couputed API

- 在Options API中，使用computed选项处理计算属性，在Composition API中，使用computed 方法编写计算属性

#### computed API有两种使用方式

- 方式一：接收一个getter函数，并为getter函数的返回值，返回一个不变的ref对象(readonly)

```js
const fullname=computed(()=> firstname.value+" "+lastname.value)    //fullname 为readonly的ref对象
```

- 方式二：接收一个汉语哦getter函数和一个setter函数的对象，返回一个可变的ref对象

```js
const fullname=computed({
    get:()=>firstname.value+" "+lastname.value,
    set:(newValue)=>{
        const names=newValue.split(" ")
        firstname.value=names[0]
        lastname.value=names[1]
    }
})
```

### watchEffect API

- 传入一个函数，响应式地追踪函数里涉及的依赖，当依赖变更时执行改函数

- 在页面加载完成时，会首先自动调用一次传入的函数

#### watchEffect 的停止侦听

- watchEffect可以返回一个stop函数，当需要停止侦听时，可以调用stop函数

```js
setup(){
    const name=ref('cyh')
    const age=ref(22)

    const changeName=()=>name.value='蔡元浩'
    const changeAge=()=>{
        age.value++
        if(age.value>25)
            stop()
    }

    const stop=watchEffect(()=>{
        console.log(name.value,age.value);
    })

    return{
        name,
        age,
        changeName,
        changeAge,
        stop
    }
}
```

#### watchEffect的执行时机

- 在Options API中，如果要获得dom中的标签元素，可以给标签的ref属性赋值，再用this.$refs.属性值获得改元素

```html
<template>
    <div>
        <h2 ref="title">Hello World</h2>
    </div>
</template>
```

在js代码中，用this.$refs.title可以得到```<h2 ref="title">Hello World</h2>```

- 但是在Composition API中无法使用this，获得title的方法

```js
import {ref,watchEffect} from 'vue'
export default{
    setup(){
        const title=ref(null)

        watchEffect(()=>{
            console.log(title.value)
        })
        return {
            title
        }
    }
}
```

title的初值为null
当html元素挂载完成时，title会赋值到title.val中

- watchEffect的第二个参数flush可以设置执行的时机。默认为pre，会提前执行；如果在侦听函数中用到DOM中的元素，希望在挂载之后执行，可以设置参数flush:"post"

```js
setup(){
    const title=ref(null)

    const show=()=>{
        console.log(title.value);
    }

    watchEffect(()=>{
        console.log(title.value)
    },{
        flush:"post"
    })

    return {
        title,
        show
    }
}
```

### 生命周期函数

- 在setup中可以直接导入onX函数注册生命周期钩子

|Options API|Hook inside setup|
|----|----|
|beforeCreate|Not needed|
|created|Not needed|
|beforeMount|onBeforeMount|
|mounted|onMounted|
|beforeUpdate|onBeforeUpdate|
|updated|onUpdated|
|beforeUnmount|onBeforeUnmount|
|unmounted|onUnmounted|
|activated|onActivated|
|deactivated|onDeactivated|

```js
import {onMounted,onUpdated,ref} from 'vue'
export default {
    setup(){
        const counter=ref(100)

        const increment=()=>{
            counter.value++
        }

        onMounted(()=>{
            console.log('Mounted')
        })

        onUpdated(()=>{
            console.log('Updated')
        })

        return{
            counter,
            increment
        }
    }
}
```

### Provide和Inject API