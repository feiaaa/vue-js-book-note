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

现在一般不使用，改用axios [https://www.kancloud.cn/yunye/axios/234845](https://www.kancloud.cn/yunye/axios/234845)