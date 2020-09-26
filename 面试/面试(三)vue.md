# vue2.0/3.0双向数据绑定的实现原理

**vue2.0 Object.defineProperty（）**

```js
   let obj = {
        name:""
    }
    // 深拷贝对原始数据进行拷贝
    let newObj = JSON.parse(JSON.stringify(obj))
    Object.defineProperty(obj,"name",{
        get(){
            //获取数据的时候返回newObj中的
            return newObj.name
        },
        set(val){
            if(newObj.name == val){return}
            newObj.name = val
            //触发观察者方法
            observer()
        }
    })
    function observer(){
        // 修改input和span中数据
        spanName.innerHTML = obj.name
        inpName.value = obj.name
    }
    inpName.oninput = function(){
        obj.name = inpName.value
    }
    setTimeout(()=>{
        obj.name = "hyj"
    },1000)
```

不好的地方：1.对原始数据克隆

2.需要分别给对象中的每一个属性设置监听0

**vue3.0 Proxy**

```js
    let obj = {}
    obj = new Proxy(obj,{
        get(target,prop){
            return target[prop]
        },
        set(target,prop,value){
            target[prop] = value
            observe(value)
        }
    })
    function observe(value){
        spanName.innerHTML = value
        inpName.value = value
    }
    inpName.oninput = function(){
        obj.name = inpName.value
    }
```

# 说一下对Vue生命周期的理解

* Vue实例从创建到销毁的过程就是vue的生命周期。
* 总共有8个阶段，`beforeCreate`,`created`,`beforeMount`,`mounted`,`beforeUpdate,`,`updated`,`beforeDestory`,`destoryed`。创建前/后，挂载前/后，更新前/后，销毁前/后。

# vue双向数据绑定的原理

* vue实现双向数据绑定主要是：采用数据劫持结合发布者-订阅者模式的方式。
* 数据劫持：通过Object.defineProperty()来劫持对象各个属性的setter，getter。
* 发布者-订阅者模式：在数据变动时发布者发布消息给订阅者，触发相应的监听回调。

# Vue如何实现参数传递

* 父传子：子通过props属性接收
* 子传父：子通过$emit方法传递参数
* 兄弟组件传值：eventBus和vuex

# `<keep-alive></keep-alive>`作用

`<keep-alive></keep-alive>`包裹动态组件时，会缓存不活动的组件实例，主要是保留组件状态或避免重新渲染。