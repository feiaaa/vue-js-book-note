# Vuetify桌面端环境搭建

**Vue开发软件和相应插件：**

- IDE：VSCode；
- VS插件：Vetur、ESLint、Auto Rename Tag、HTML CSS Support、Debugger for Chrome、Path Intellisense、Prettier-Code formatter、vscode-icons
- Chrome插件：VUE Devtools

**相关链接：**

- Node.js官网：[https://nodejs.org/en/](https://nodejs.org/en/)
- Vue.js官网：[https://cn.vuejs.org/](https://cn.vuejs.org/)
- Vuetify官网：[https://vuetifyjs.com/zh-Hans/](https://vuetifyjs.com/zh-Hans/)
- VSCode下载：[https://code.visualstudio.com/](https://code.visualstudio.com/)

**1.安装node.js**

> 命令行输入 `node -v`能看到版本号即安装成功

**2.创建vue项目**

> 先在要创建项目到的地方建立相应文件夹，假设对应路径为`E:\karaio`

```
cd E:\karaio
npm install --global vue-cli
vue init webpack my-peject			//my-project是你项目的名字
(此处cmd面板会有一些选项，直接按Enter键就行。Y/n选项时可以选择安装'vue-router'其他全部选n)
npm run dev
```

**提示：**如果创建项目时没安装`vue-router`，那么接下来的配置中就不会存在index.js文件。(并不会对项目造成影响)

至此，浏览器访问`localhost:8080`就能看到项目创建成功

**3.在vue项目上加入Vuetify**

```
// 退出项目运行状态 Ctrl+C 然后按 Y 

// 在当前项目下继续输入命令
E:\karaio\my-project> npm install vuetify --save

// 在index.js或者main.js中添加
import Vuetify from 'vuetify'
import 'vuetify/dist/vuetify.min.js'
import 'vuetify/dist/vuetify.min.css'
Vue.use(Vuetify)

// 在index.html添加图标字库
<head>
	<link href='https://fonts.googleapis.com/css?family=Roboto:300,400,500,700|Material+Icons' rel="stylesheet">
</head>
```

**4.创建自己的vue文件**

1.在components文件夹下新建`MyFirstVuetify.vue`，并粘贴Vuetify官网的组件模板;

2.在index.js或main.js中添加

```
import MyFirstVuetify from '@/components/MyFirstVuetify'

export default new Router({
  mode: 'history',
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    },{
      path: '/vuetify',
      name: 'MyFirstVuetify',
      component: MyFirstVuetify
    }
  ]
})
```

**提示：**在main.js中import时好像不能使用`@`，换成`.`即可。

3.命令`npm run dev`运行项目，浏览器访问`localhost:8080/vuetify`