---
layout: post
title:  "Vue基础知识点"
categories: Java
tags:  Java
author: FTX
---

* content
{:toc}

### 1 Vue 介绍

Vue.js（读音 /vjuː/, 类似于 view） 是一套构建用户界面的渐进式框架(mvvm架构设计)。

m: 数据  v：视图   vm （数据视图）

Vue 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件(数据双向绑定)。

 

**入门案例**



```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="js/vue.js"></script>
	</head>
	<body>
			
		<div id="myApp">
			<input type="text" v-model="name" />
			<span>name值是:{{name}}</span>
		</div>
	</body>
	<script>
		 new Vue({
			 el:"#myApp",//vue的在页面中的作用范围
			 data:{
				 name:'222'
			 }
		 })
	</script>
</html>


```



## 1.1环境准备



 1 以下推荐国外比较稳定的两个 CDN，国内还没发现哪一家比较好，目前还是建议下载到本地。

Staticfile CDN（国内） : https://cdn.staticfile.org/vue/2.2.2/vue.min.js



cdnjs : https://cdnjs.cloudflare.com/ajax/libs/vue/2.1.8/vue.min.js

 

2、NPM 安装，一般用于前后端分离开发模式

### 2 Vue 模板语法

Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。

## 2.1绑定文本

数据绑定最常见的形式就是使用 {{...}}（双大括号）的文本插值：

```html
<div id="app">
  <p>{{ message }}</p>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    message: 'Hello world!'
  }
})
</script>
```



 

## 2.2绑定HTML

使用 v-html 指令用于输出 html 代码：
```
<div id="app">
    <div v-html="message"></div>
</div>
	
<script>
new Vue({
  el: '#app',
  data: {
    message: '<h1>Hello world</h1>'
  }
})
</script>

```



## 2.3绑定HTML属性

参数在指令后以冒号指明。例如， v-bind 指令被用来响应地更新 HTML 属性

```
<div id="app">
    <pre><a v-bind:href="url">百度</a></pre>
</div>
	
<script>
new Vue({
  el: '#app',
  data: {
    url: 'http://www.baidu.com'
  }
})
</script>

```



## 2.4绑定表单元素

v-model 指令用来在 input、select、textarea、checkbox、radio 等表单控件元素上创建双向数据绑定，根据表单上的值，自动更新绑定的元素的值

```
<input type="text" v-model="info">
```


## 2.5缩写

**1 v-bind** **缩写**

Vue.js 为两个最为常用的指令提供了特别的缩写：

```
<!-- 完整语法 -->

<a v-bind:href="url"></a>

<!-- 缩写 -->

<a :href="url"></a>
```

**2  v-on** **缩写**
```
<!-- 完整语法 -->

<a v-on:click="doSomething"></a>

<!-- 缩写 -->

<a @click="doSomething"></a>
```

 

## 2.6 v-show指令

v-show指令用来控制html元素是否显示
```

<div id="app">
    <h1 v-show="ok">Hello!</h1>
    
    <button @click="ss">控制HTML元素显示</button>
</div>
	
<script>
new Vue({
  el: '#app',
  data: {
    ok: true
  },
  methods:{
  	ss:function(){
  		this.ok =!this.ok
  	}
  }
})
</script>	

```



### 3 Vue条件语句

条件判断使用 v-if  v-else-if v-else指令：
```
<div id="app">
    <div v-if="type == 'A'">
      A
	</div>
	<div v-else-if="type == 'B'">
	  B
	</div>
	<div v-else-if="type == 'C'">
	  C
	</div>
	<div v-else>
	  Not A/B/C
	</div>
</div>
	
<script>
new Vue({
  el: '#app',
  data: {
    type: 'A'
  }
})
</script>

```





### 4 Vue循环语句

循环使用 v-for 指令。

v-for 指令需要以 x in sites 形式的特殊语法， sites 是源数据数组并且 x 是数组元素迭代的别名。

v-for 可以绑定数据到数组来渲染一个列表：

 

```

<div id="app">
	<table>
		<thead>
				<th>id</th>
				<th>name</th>
		</thead>
		<tbody>
			  <tr v-for="x in sites">
			  	<td>  {{ x.id }}</td>
			  	<td>  {{ x.name }}</td>
			  </tr>
		</tbody>
	</table>
</div> 
<script>
new Vue({
  el: '#app',
  data: {
    sites: [
      { id:1,name: 'cat' },
      { id:2, name: 'pig' },
      { id:3, name: 'dog' }
    ]
  }
})
</script>

```

 

注意：如果要获取数据索引值，v-for可以这样写：v-for="(x,index) in sites"

Index就是数据索引值(0开头)

 

 

 

 

### 5 Vue事件处理

## 5.1 v-on 用法

v-on或者缩写@ 可以接收一个定义的方法来调用。

```
<div id="app">
	<button @click="login">登录</button>
</div>
 
<script>
new Vue({
  el: '#app',
  methods: {
	  login:function(){
	  	 alert("登录")
	  }
	  
  }
})
</script>

```

 

## 5.2 方法传参

 方法参数传递，和javascript一样可以直接传参
```
<div id="app">
	<button @click="login(1)">登录</button>
</div>
<script>
new Vue({
  el: '#app',
  methods: {
	  login:function(data){
	  	 alert("登录"+data)
	  }
	  
  }
})

```

 

 

## 5.3 文档事件

3 如果要函数需要在文档加载完后执行，需要定义mouted属性 代码如下：

```
<script>
new Vue({
  el: '#app',
  methods: {
	  login:function(data){
	  	 alert("登录"+data)
	  }
  },
  mounted:function(){
  	this.login(1)
  }
})
</script>

```



1. **created**:
   - `created`生命周期钩子在Vue实例被创建后立即调用，实例的数据观测（data observer）和事件/生命周期事件监听器都被初始化了，但是DOM元素和其它子组件尚未被挂载。
   - 这个阶段适合进行一些初始化操作，比如数据的初始化、事件的监听等。但是如果需要操作DOM元素，则应该在`mounted`阶段进行。
2. **mounted**:
   - `mounted`生命周期钩子在Vue实例的根DOM元素成功挂载到页面后被调用。
   - 在这个阶段，Vue实例已经完成了DOM元素的挂载，可以对DOM进行操作。通常用来执行一些需要DOM的操作，如初始化第三方插件、设置定时器等。



### 6 Vue表单处理

## 6.1输入框

input 和 textarea 元素中使用 v-model 实现双向数据绑定，前面绑定表单元素已经有案例，

这里就贴案例了

 

## 6.2复选框

复选框也是使用 v-model 实现双向数据绑定，需要注意的是定义的值是一个js数组,

代码如下：

```
  <p>多个复选框：</p>
  <input type="checkbox"  value="电影" v-model="checkedNames">
  <label >电影</label>
  <input type="checkbox" value="游戏" v-model="checkedNames">
  <label >游戏</label>
  <input type="checkbox"  value="看书" v-model="checkedNames">
  <label >看书</label>
  <br>
  <span>选择的值为: {{ checkedNames }}</span>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    checkedNames: []//定义一个数组
  }
})

```

## 6.3单选框

```
<div id="app">
  <input type="radio" value="男" v-model="sex">
  <label >男</label>
  <br>
  <input type="radio"  value="女" v-model="sex">
  <label >女</label>
  <br>
  <span>选中值为: {{ sex }}</span>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    sex : '男'
  }
})
</script>

```

## 6.4下拉框

以下实例中演示了下拉列表的双向数据绑定：

```
<div id="app">
  <select v-model="selected" >
    <option value="">选择一个网站</option>
    <option value="www.baidu.com">百度</option>
    <option value="www.google.com">谷歌</option>
  </select>
 
  <div id="output">
      选择的网站是: {{selected}}
  </div>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    selected: '' 
  }
})
</script>

```





### 7  计算属性

计算属性关键词: computed。

计算属性在处理一些复杂逻辑时是很有用的。



例如：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="js/vue.js"></script>
	</head>
	<body>
			
		<div id="myApp">
			<span>{{myMoney}}</span>
			<span>{{myTime}}</span>
		</div>
	</body>
	<script>
		 new Vue({
			 el:"#myApp",//vue的在页面中的作用范围
			 data:{
				 money:30,
				 time:new Date()
			 },
			 computed:{
				 myMoney:function(){
					 return this.money+"$"
				 },
				 myTime:function(){
					 var y = this.time.getFullYear();
					 var m = this.time.getMonth();
					 var d = this.time.getDay();
				 	return y+"年"+(m+1)+"月"+d+"日";
				 }
			 }
		 })
	</script>
</html>

```

组件,路由