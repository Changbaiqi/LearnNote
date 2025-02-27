---
title: Vue3学习
date: 2023-12-28 14:49:56
author: 长白崎
categories:
  - "前端"
tags:
  - "Vue"
---



# Vue3学习

---

## 创建Vue3.0工程

### 1.使用vue-cli创建

```
## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version
## 安装或者升级你的@vue/cli
np m install -g @vue/cli
## 创建
vue create vue_test
## 启动
cd vue_test
npm run serve
```



### 2.使用vite创建

```
## 创建工程
npm init vite-app <project-name>
## 进入工程
cd <project-name>
## 安装依赖
npm install
## 运行
npm run dev
```



### 3.设置npm代理本地clash

```shell
npm config set proxy http://proxy.example.com:8080
npm config set https-proxy http://proxy.example.com:8080
```





setup函数的两种返回值：

> 1.若返回一个对象，则对象中的属性、方法，在模板中均可以直接使用。（重点关注！）
>
> 2.若返回一个渲染函数：则可以自定义渲染内容。（了解）



注意点：

> 1、尽量不要与Vue2.x配置混用
>
> * Vue2.x配置（data、methos、computed...)中可以访问到setup中的属性、方法。
> * 但在setup中不能访问到Vue2.x配置（data、methos、computed...)。
> * 如果有重名，setup优先。
>
> 2、setup不能是一个async函数，因为返回值不再是return的对象，而是promise，模板看不到return对象中的属性。



## 2.ref函数

* 作用：定义一个响应式的数据

* 语法：==const xxx = ref(initValue)==

* 创建一个包含响应式数据的引用对象（reference对象）。

* JS中操作数据：==xxx.value==

* 模板中读取数据：不需要.value,直接``<div>{{xxx}}</div>``

* 备注：

  * 接受的数据可以是：基本类型、也可以是对象类型

  * 基本类型的数据：响应式依然是靠``Object.defineProperty()``的``get``与``set``完成的。
  * 对象类型的数据：内部``“求助”``了Vue3.0中的一个新函数——==reactive==函数。

```vue
<template>
<h1>一个人的信息</h1>
<h2>姓名：{{name}}</h2>
<h2>年龄：{{age}}</h2>
<h3>工作类型：{{job.type}}</h3>
<h3>工作薪水：{{job.salary}}</h3>
<button @click="sayHello">说话</button>
</template>

<script>
    import {ref} from 'vue'

    export default {
        name: 'App',
        //此处只是测试一个车setup，暂时不考虑响应式的问题。
        setup(){
            let name = ref('张三')
            let age = ref(18)
            let job = ref({
                type: '前端',
                salary: '30K'
            })

            function sayHello(){
                name.value = '李四';
                age.value= 20;
                job.value.type='后端'
                job.value.salary='40K'
            }

            //返回一个对象（常用）
            return{
                name,
                age,
                job,
                sayHello
            }
        }
    }
</script>

<style scoped>

</style>
```

其中需要监听的数据用ref来包装一下，之后如果想要修改那么就使用xx.value去引用修改即可。注意不能直接进行xxx=这样直接修改，不然不会更新界面数据的。





## 3.reactive函数

* 作用：定义一个对象类型的响应式数据（基本类型别用它，用==ref==函数）
* 语法：==const代理对象=reactive(被代理对象)==接收一个对象（或数组），返回一个代理对象（proxy对象）
* reactive定义的响应式数据是“深层次的”。
* 内部基于ES6的Proxy实现，通过代理对象操作源对象内部数据都是响应式的
* 在使用的时候就不需要使用``.value``进行修改和引用了

## 4.Vue3.0中的响应式原理

* 实现原理：

  * 对象类型：通过``Object.defineProperty()``对属性的读取，修改进行拦截（数据劫持）。

  * 数据类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）。

    ```js
    Object.defineProperty(data,'count',{
        get(){},
        set(){}
    })
    
    ```

* 存在问题：
  * 新增属性、删除属性，界面不会更新。
  * 直接通过下标修改数组，界面不会更新。



Vue3.0的响应式

* 实现原理：

  * 通过Proxy（代理）：拦截对象中任意属性的变化，包裹：属性值的读写、属性的添加、属性的删除等。

  * 通过Reflect（反射）：对被代理对象的属性进行操作。

  * MDN文档中描述的Proxy与Reflect：

    * Proxy：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy

    * Reflect：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

      ```javascript
      new Proxy(data,{
          get(target,prop){
              return Reflect.get(target,prop)
          },
          //拦截设置属性值或添加新属性
          set (target,prop.value){
              return Reflect.set(target,prop,,value)
          },
          //拦截删除属性
          deleteProperty(target,prop){
              return Reflect.deleteProperty(target,prop)
          }
      })
      proxy.name = 'tom'
      ```

      



## 6.setup的两个注意点

* setup执行的时机
  * 在beforeCreate之前执行一次，this是undefined。

* setup的参数
  * props：值为对象，包含：组件外部传递过来，且组件内部申明接收了的属性。
  * context：上下文对象
    * attrs：值为对象，包含：组件外部传递过来，但没有在props配置中申明的属性，相当于``this.$attrs``。
    * slots：收到的插槽内容，相当于``this.$slots``。
    * emit：分发自定义事件的函数，相当于``this.$emit``。



## 7.计算属性与监视

### 1.computed函数

* 与Vue2.x中computed配置功能一致

* 写法

  ```js
  import {computed} from 'vue'
  setup(){
      ...
      //计算属性一简写
      let fullName = computed(()=>{
          return person.firstName+'-'+person.lastName
      })
      //计算属性完整
      let fullName = computed({
          get(){
              return person.firstName()+'-'+person.lastName
          },
          set(value){
              const nameArr = value.split('-')
              person.firstName = nameArr[0]
              person.lastName = nameArr[1]
          }
      })
      }
  ```





在vue3中如果使用template插槽，尽量使用v-slot形式，比如``v-slot:qwe``







### 2.watch函数

* 与Vue2.x中watch配置功能一致

* 两个小“坑”：

  * 监视reactive定义的响应式数据时：oldValue无法正确获取、强制开启了深度监视（deep配置失效）。
  * 监视reactive定义响应式数据中某个属性时：deep配置有效。

  ```js
  //情况一：监视ref定义的响应式数据
  watch(sum,(newValue,oldValue)=>{
      console.log('sum变化了',newValue,oldValue)
  },{immediate: true})
  
  //情况二：监视多个ref定义的响应式数据
  watch([sum,msg],(newValue,oldValue)=>{
      console.log('sum或msg变化了',newValue,oldValue)
  })
  
  /* 情况三：监视reactive定义的响应式数据
  			若watch监视的是reactive定义的响应式数据，则无法正确获取oldValue！！
  			若watvh监视的是reactive定义的响应式数据，则强制开启了深度监视
  */
  watch(person，(newValue,oldValue)=>{
      console.log('person变化了',newValue,oldValue)
  },{
      immediate:true,deep:false
  })//此处的deep配置不再凑效
  
  //情况四：监视reactive定义的响应式数据中的某个属性
  watch(()=>person.job,(newValue,oldValue)=>{
      console.log('person的job变化了',newValue,oldValue)
  },{immediate:true,deep:true})
  
  //情况五：监视
  ```

  