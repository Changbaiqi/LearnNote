# VueX学习

---



## VueX是什么？

概念：专门在Vue中实现几种式状态管理（数据）管理的一个Vue插件，对Vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通讯的方式，且适用于任意组件间通信。



## 什么时候使用VueX

1.多个组件依赖于同一状态

2.来自不同组件的行为需要变更同一状态

![](Img\Vue\VueX多组件共享数据图.png)

VueX原理图：

![](Img\Vue\VueX原理图.png)



## VueX的安装

---

如果直接使用指令：

```
npm i vuex
```

这样使用就会有问题，因为现在Vue默认会使用Vue3版本的VueX，所以直接下载的话会下载vueX的最新版本VueX4版本。因为我们现在是Vue2所以要下载VueX3版本。

所以要用指令指定版本：

```
npm i vuex@3
```

然后再main.js引入vuex：

```js
mport Vue from 'vue';
import App from './App.vue';
import Vuex from 'vuex';

//使用插件
Vue.use(Vuex)

new Vue({
  render: h => h(App),
}).$mount('#app')

```



### 搭建VueX环境

---

1.创建文件：src/store/index.js

```js
//引入Vue核心库
import Vue from 'vue'
//引入VueX
import Vuex fom 'vuex'
//应用Vuex插件
Vue.use(Vuex)

//准备actions对象——响应组件中用户的动作
const actions={}
//准备mutations对象——修改state中的数据
const mutations={}
//准备state对象——保存具体的数据
const state={}

//创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state
})
```

main.js引入：

```js
import store from './store'
new Vue({
    el:'#app',
    render: h=>(App),
    store
})
```



### 基本使用

初始化数据、配置actions、配置mutations、操作文件store.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
const actions = {
        //响应组件中加的动作
        jia(context,value){
            //console.log('actions中的jia被调用了',miniStore,value)
            context,commit("JIA",value)
        },
    }
 
const mutations = {
    //执行加
    JIA(state,value){
        //console.log('mutations中的JIA被调用了',miniStore,value)
        state.sum += value
    }
}

//初始化数据
const state = {
    sum: 0
}

//创建并暴露store
export default new Vuex.Store(
    actions,
    mutations,
    state,
)
```

组件中读取vuex中的数据：\$store.state.sum

组件中修改vuex中的数据：\$store.dispatch('action中的方法名',数据)或\$srore.commit('mutations中的方法名',数据)

备注：若没有网络请求或其他业务逻辑，组件中也可以越过actions，即不写dispatch，直接编写commit



