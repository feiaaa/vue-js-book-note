## 7、Vue.js常用插件

### 7.1 vue-router

>  路由管理

1. 使用方式

```javascript
const router = new VueRouter({
    // 路由规则在实例化VueRouter的时候就直接传入
    routes: [
        {
            path: '/biz',
            component: Biz,
            children: [
                {
                    path: 'list',//不用加'/'，否则认为从根路径开始
                    component： List
                }
            ]
        }
    ]
})
```

2. 跳转方式

```html
<router-link to="/home">Home</router-link>
```

3. 钩子函数

全局钩子：`beforeEach` 和 `afterEach` ，在每个路由切换前/后调用。

单个钩子：`beforeEnter` 需要在配置路由时直接定义。

组件内钩子：`beforeRouteEnter` `beforeRouteLeave` 在组件内定义。

4. 获取数据

在组件的create()钩子函数和watch: {route: ''}中调用获取数据的函数。

```javascript
export default{
    data(){
        return {
            ...
        }
    },
    created(){
        //组件创建完后获取数据
        this.fetchData()
    },
    watch: {
        //如果路由有变化，会再次执行该方法
        '$route': 'fetchData'
    },
    methods: {
        fetchData(){
            //调用异步请求获取数据
        }
    }
}
```

在导航获取之前完成数据，可以在beforeRouteEnter钩子中获取数据，并且只有当数据获取成功或确定有权限之后才渲染组件，否则就回退到路由变化前组件状态。

5. 命名视图

多个视图并列显示时

```
<router-view></router-view>
<router-view name='main'></router-view>

配置路由时path下面使用components
```



### 7.2  vue-resource

>  数据请求

- 引用: npm install或者script
- 使用：
```
var List = Vue.extend({
route : {
　　// vue-router 中的 data 钩子函数，
　　data : function(transition) {
　　　// 运行这段代码需要在服务器环境中，即 localhost 下，直接访问文件运行这段代码会抛出异常
　　　this.$http
　　　　　.get('/api/list?pageNo=' +  + transition.to.params.page);
　　　　　.then(function(rep){
　　　　　　 // 成功回调函数
　　　　　　 transition.next({
　　　　　　　 list : rep.data
　　　　　　 });
　　　　　}, function(rep) {
　　　　　　 // 失败回调函数
　　　　　　 transition.next({
　　　　　　　 data : rep.data
　　　　　　 });
　　　　　});
　　　}
},
template: '<h1>This is the list page</h1>'
})
```
> 选项参数
```
this.$http({
 url : '/api/list', // url 访问路径
 method : '',　　 // HTTP 请求方法，例如 GET,POST,PUT,DELETE 等
 body : {},　　　// request 中的 body 数据，值可以为对象，String 类型 , 也可以是
FormData 对象 
params : {}，　 // get 方法时 url 上的参数，例如 /api/list?page=1
 headers: {},　　 // 可以设置 request 的 header 属性
 timeout : 1500, // 请求超时时长，单位为毫秒，如果设置为 0 的话则没有超时时长
 before : function(request) {}, // 请求发出前调用的函数，可以在此对 request进行修改

```
- Vue-resource 提供了一种与 RESTful API 风格所匹配的写法，通过全局变量 Vue.resource 或者组件实例中的 this.$resource 对某个符合 RESTful 格式的 url 进行封装，使得
开发者能够直接使用增删改查等基础操作，而不用自己再额外编写接口。
> Vue-resource 提供了 6 个默认动作行为，分别为：
```
get: {method: 'GET'},
save: {method: 'POST'},
query: {method: 'GET'},
update: {method: 'PUT'},
remove: {method: 'DELETE'},
delete: {method: 'DELETE'}
```

- 封装service
> 在编写 SPA 应用中，我们通常会把和后端做数据交互的方法封装成一个 Service 模块，供不同的组件进行使用。我们可以新建一个文件夹 api，将 Service 模块集中起来，并按资源进行分类。

现在改用axios [https://www.kancloud.cn/yunye/axios/234845](https://www.kancloud.cn/yunye/axios/234845)