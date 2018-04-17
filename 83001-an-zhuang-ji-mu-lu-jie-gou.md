## 8、Vue.js工程实例

### 8.1 准备工作

webpack和vue-loader进行模块化开发，代码编译和打包。

webpack：是一款模块加载及处理工具，把各种资源都作为模块使用和处理。

vue-loader：是webpack的一个加载器，处理.vue文件

### 8.2 安装及目录结构

```
npm install -g vue-cli		//全局环境
vue init webpack my-peject	//生成项目
npm run dev				  //运行项目
```

生成目录：

- build：webpack相关配置和脚本
- config：存放配置文件，用于区分开发环境、测试环境、线上环境的不同
- src：项目源码及需要引入的资源文件
- static：不需要webpack处理的静态资源
- test：存放测试文件