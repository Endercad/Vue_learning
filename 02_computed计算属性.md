
### 复杂data的处理

#### 1.在模板中使用表达式，但是背离了用于简单计算的初衷

#### 2.将逻辑抽取到method中，弊端是data的使用过程变成了方法的调用

#### 3.使用computed
---
### coumuted计算属性
#### 对于任何包含响应式数据的复杂逻辑，都应该使用计算属性
#### 计算属性将被混入到组件实例中，所有的getter和setter的this上下文自动的绑定为组件实例
---
### 计算属性的用法
#### 写法1.和method写法一样
#### 写法2.getter和setter
```
computed:{
    fullname:function(){
        return this,firstname+" "+this.lastname;
    }
    //默认为getter方法

    //getter和setter方法 
    fullname:{
        get:function(){

        },
        set:function(){

        }
    }
}
```

---
### 计算属性和method的区别：计算属性有缓存，多次调用实际上只计算了一次，method没有缓存，调用几次计算几次