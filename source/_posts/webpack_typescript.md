---
title: typescript+webpack基础开发配置
date: { date }
tags: webpack typescript
---

1、安装相应的依赖
```
npm install ts-loader --save-dev
```
2、webpack.base.config添加如下配置
```javascript
{
    test: /\.tsx?$/, // typescript配置
    loader: 'ts-loader',
    include: resolve('src'),
    exclude: /node_modules/
},
```
3、入口文件改成ts文件
4、根目录添加tsconfig.json文件
```javascript
{
    "compilerOptions": {
      "outDir": "./dist/",
      "sourceMap": true,
      "noImplicitAny": true,
      "module": "es6",
      "target": "es5",
      "jsx": "react",
      "allowJs": true
    }
}
```