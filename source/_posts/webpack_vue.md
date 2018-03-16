---
title: 一些vue+webpack基础开发配置
date: { date }
tags: webpack vue
---

1、安装相应的依赖
```
npm install vue-loader vue-template-compiler --save-dev
```
2、webpack.base.config添加如下配置
```javascript
{
    test: /\.vue$/,
    use: ['vue-loader'],
},
```
3、src添加main.js  App.vue文件(根据自己需要决定)
4、main.js添加如下
```javascript
import Vue from 'vue';
import App from './App.vue';

new Vue({ // eslint-disable-line
	el: '#test', // index.html文件的根元素
	render: h => h(App)
});
```
App.vue
```javascript
<!--渲染模版-->
<template>
  <h1>{{ msg }}</h1>
</template>

<!--样式描述-->
<style scoped>
  h1 {
	color: red;
  }
</style>

<!--组件逻辑-->
<script>
  export default {
	data() {
		return {
			msg: 'Hello,Webpack'
		};
	}
  };
</script>
```