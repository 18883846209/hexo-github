---
title: 一些react+webpack基础开发配置
date: { date }
tags: webpack react
---

1、安装相应的依赖
```
npm install babel-preset-react react react-dom --save-dev
```
2、webpack.base.config添加如下配置
```javascript
{
    test: /\.jsx?$/,
    loader: 'babel-loader?cacheDirectory=true', // 开启缓存
    include: resolve('src'),
    query: {
        presets: ['react']
    },
    exclude: /node_modules/
},
```
3、src目录添加main.js  react.js文件(根据自己需要)
4、main.js添加如下
```javascript
import 'babel-polyfill';
import React from 'react';
import { render } from 'react-dom';
import Component from './react';

render(<Component />,document.getElementById('test')); // eslint-disable-line
```
react.js
```javascript
import React from 'react';

class Component extends React.Component{ // eslint-disable-line
	render() {
		return <div>Helllo World</div> // eslint-disable-line
	}
}
export default Component;
```