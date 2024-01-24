---
title: Flutter基础学习笔记
date: 2023-02-24 09:46:09
author: 长白崎
categories:
  - "前端"
tags:
  - "Android"
  - "Flutter"
---



# Flutter基础学习

目录结构

> android：用于android软件的相关
>
> ios：用于ios相关
>
> lib：Flutter代码存放地
>
> test：测试用例代码存放地

Dart以main函数为入口，软件以runApp实体类为软件绘制入口。

> ```dart
> import 'package:flutter/material.dart';
> 
> void main(){
>     runApp(
>     	Center(
>         child: Text('XXX'),
>         )
>     );
> }
> ```

可以单独将Center抽离成一个组件：

> ```dart
> import 'package:flutter/material.dart';
> 
> void main(){
>  runApp(MyApp());
> }
> 
> class MyApp extends StatelessWidget{
>     
>     @override
>     Widget build(BuildContext context){
>         
>            return MaterialApp(
>       home: Scaffold(
>         appBar: AppBar(
>         title: Text('Flutter Demo')
>         ),
>         body: HomeContext(),
>       ),
>       theme: ThemeData(
>         primarySwatch: Colors.deepOrange
>       ),
>     );
> 
>     }
> }
> 
> ```
>
> 

ClipOval类：

> child: Image.asset("images/b.webp") 加载本地图片
>
> width: 设置宽
>
> height: 设置高

静态组件更新：

```Dar
```



GridView网格布局：

动态组件用这个：

```dart
@override
Widget build(BuildContext context){
	return GridView.builder(
	)
}
```



静态组件用这个：

```dart
@override
Widget build(BuildContext context){
	return GridView.count(
	)
}
```





Row：

> mainAxisAlignment
>
> crossAxisAlignment
>
> children