# Vuex

## 什么是Vuex

Vuex是一个专门为Vue开发的**状态管理模式库**，它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式变化

## 为什么使用Vuex

当应用遇到多个组件共享状态是，单项数据流的间接性很容易破坏：

- 多个视图依赖于同一个状态（传参的方法对多层嵌套的组件会非常繁琐，兄弟组件之间的状态传递无法实现）

- 来自不同视图的行为需要变更同一状态（经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝）

以上这些模式非常脆弱，会导致无法维护的代码

因此，把组件的共享状态抽取出来，以一个全局单例模式管理

## Vuex安装

```npm install vuex@next```

## 核心概念

### State

Vuex使用单一状态树——即用一个对象包含全部的应用层级状态。它作为一个**唯一数据源（SSOT）**。每个应用仅仅包含一个store实例。

Vuex 的状态存储是响应式的

Vuex中的数据和Vue中的data遵循相同的规则

#### 创建store

```js
import { createStore } from 'vuex'

const store=createStore({
    state(){
        return{
            counter:100,
        }
    }
})

export default store
```

#### 在Vue组件中获得Vuex状态

- 通过{{$store.state.counter}}从store中读取状态

- 通过computed计算属性

```js
import store from '../store'
export default {
    computed:{
        counter(){
            return store.state.counter
        }
    }
}
```

#### mapState辅助函数

如果一个组件需要获取多个状态，使用计算属性则会出现重复和冗余，为了解决这个问题，可以使用**mapState辅助函数帮助生成计算属性**

```js
computed:mapState({
    //使用箭头函数比较简洁
    counter:state=>state.counter,
    //箭头函数传字符串参数，相当于 state=>state.myname
    myname:'myname',
    //也可以使用state的状态自定义数据
    doubleCounter(state){
        return state.counter*2
    }
})
```

也可以使用对象展开运算符'...'，将mapState与局部计算属性混合

```js
computed:{
    ...mapState({

    }),
    localComputed(){
        
    }
    ]
}
```

### Getter

有时候我们需要从store中的state中派生出一些状态，Vuex允许我们在store中定义getter，（可以认为是store中的计算属性）

#### getter接受state作为其第一个参数  

```js
getters:{
    doubleCounter:(state)=>{
        return state.counter*2
    }
}
```

通过{{$store.getters.doubleCounter}}可以获取getter中的状态

#### getter接受getter作为第二个参数

```js
getters:{
    doubleCounter:(state)=>{
        return state.counter*2
    },
    fourfoldCounter:(state,getters)=>{
        return getters.doubleCounter+state.counter*2
    }
}
```

#### mapGetters

mapGetters辅助函数将store中的getter映射到局部计算属性

数组写法

```js
computed:{
    ...mapGetters([
        'fourfoldCounter',  //则将生成fourfoldCounter计算属性，名字取决于getters中的状态
    ])
}
```

对象写法

```js
computed:{
    ...mapGetters({
        fourfold:'fourfoldCounter', //可以将getter属性另取名字
    })
}
```

### Mutation

更改Vuex的store中的状态**唯一方法是提交Mutation**。Vuex中的Mutation非常类似于事件。每个mutation都有一个字符串的事件类型和一个回调函数。这个回调函数救赎我们实际进行状态更改的地方，并且他会接受state作为第一个参数,如果组件传入参数，则会接受payload作为第二个参数

#### 注册mutation

```js
mutations:{
    increment(state,payload){
        state.counter+=payload.amount;
        console.log(payload);
    }
}
```

在组件中不能直接调用mutation处理函数，而是要以相应的type调用store.commit()

```js
store.commit('increment',{
    amount:10
})
```

#### mapMutations

辅助函数mapMutations将组件中的methods映射为store.commit()调用

```js
methods:{
    //数组写法
    ...mapMutations([
        'increment',    //将this.increment()映射为this.$store.commit('increment')
    ])

    //对象写法
    ...mapMutations({
        add:'increment',    //将this.add()映射为this.$store.commit('increment')
    })

}
```

在Vuex中，mutation都是**同步事务**

### Action

action类似于mutation，不同在于：
action提交的是mutation，而不是直接变更状态
action可以包含任意异步操作

#### action注册

```js
actions:{
    incrementAction(context){
        context.commit('increment')
    }
}
```

#### 分发action

action通过store.dispatch方法触发
```store.dispatch('increment')```

通过使用action，可以在action内部执行异步操作

```js
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

#### 在组件中分发action

在组件中使用this.$store.dispatch('xxx')分发action，或者使用mapActions辅助函数将组件的methods映射为store.dispatch

### Module