

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
* * 