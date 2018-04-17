# Vux移动端环境搭建

> 在学习之前需要有html、css、JavaScript、jQuery的基础知识

**使用软件：**

VsCode以及相关插件 `Vetur`

## 1、学习资料

### 1.1 ECMAScript 6

> ECMAScript 6是对JavaScript语法的改进，需要熟悉新的标准的使用。（js语法）

- [ECMAScript 6 入门](http://es6.ruanyifeng.com/)
- [菜鸟教程](http://www.runoob.com/w3cnote/es6-concise-tutorial.html)
- [W3CSchool](http://www.w3school.com.cn/js/pro_js_syntax.asp)

### 1.2 webpack

> webpack 是一个现代 JavaScript 应用程序的静态模块打包器（打包项目）

- [webpack中文网](https://www.webpackjs.com/concepts/)
- [菜鸟教程](https://www.runoob.com/w3cnote/webpack-tutorial.html)
- [SegmentFault](https://segmentfault.com/a/1190000011530762)
- [W3CSchool](https://www.w3cschool.cn/webpackguide/)

### 1.3 Vue.js

> WebApp的开发框架

- [Vue官网](https://cn.vuejs.org/)
- [菜鸟教程](http://www.runoob.com/w3cnote/vue2-start-coding.html)
- [Vuex](https://vuex.vuejs.org/zh-cn/) 数据共享组件
- [Element UI](http://element-cn.eleme.io/#/zh-CN) Vue2.0桌面端组件
- [VUX](https://doc.vux.li/zh-CN/about/before-using-vux.html) Vue2.0移动端组件

## 2、开发环境配置

1. 安装[node.js](https://nodejs.org) `node -v`
2. 更改npm为淘宝cnpm `$ npm install -g cnpm --registry=https://registry.npm.taobao.org`
3. 安装vue支持库
4. [基础操作](https://www.cnblogs.com/dreling/p/6892684.html)

**注意：(否则路由不起作用)：**

```javascript
const router = new VueRouter({
  mode: 'history',
  routes
})
```

### 2.1 基于vue搭建vux项目

[VUX](https://doc.vux.li/zh-CN/about/before-using-vux.html) Vue2.0移动端组件

1. 安装node.js：直接在官网下载安装（使用`node -v`查看安装是否成功）
2. 安装Vue：`npm install --global vue-cli`
3. 使用淘宝镜像：`npm install -g cnpm --registry=https://registry.npm.taobao.org`
4. 安装Vux： `cnpm install vux --save`
5. 在文件中(projectPath)创建项目：`vue init airyland/vux2`
6. `cnpm install`
7. `npm run dev`

**备注：**

- 再次创建项目时可直接从第5步开始
- 创建项目时可全部选否(n)
- 报错可能是开启了ESLint验证，可以关闭