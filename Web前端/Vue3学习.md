

# Vue3学习

---

## 创建Vue3.0工程

> 1.使用vue-cli创建

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



> 2.使用vite创建

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



> 2.ref函数

* 作用：定义一个响应式的数据

* 语法：==const xxx = ref(initValue)==

* 创建一个包含响应式数据的引用对象（reference对象）。

* JS中操作数据：==xxx.value==

* 模板中读取数据：不需要.value,直接==<div>{{xxx}}</div>==

* 备注：

  * 接受的数据可以是：基本类型、也可以是对象类型

  * 基本类型的数据：响应式依然是靠==Object.defineProperty()==的==get==与==set==完成的。
  * 对象类型的数据：内部**“求助”**了Vue3.0中的一个新函数——==reactive==函数。

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





> 3.reactive函数

* 作用：定义一个对象类型的响应式数据（基本类型别用它，用==ref==函数）
* 语法：==const代理对象=reactive(被代理对象)==接收一个对象（或数组），返回一个代理对象（proxy对象）
* reactive定义的响应式数据是“深层次的”。
* 内部基于ES6的Proxy实现，通过代理对象操作源对象内部数据都是响应式的

> 4.Vue3.0中的响应式原理