## 2、基础特性

### 2.1 实例及选项

#### 2.1.1 模板

* Vue.js实例必须只有一个根div，即组件模板的内容必须封装在一个div里面。

```
<script src="../js/vue.js"></script>

<div id="app">
    <!-- 内容会被模板覆盖 -->
    <p>123</p>
</div>

<script id="tp1" type="x-template">
    <div>
        <p>模板</p>
    </div>
</script>

<script>
/* 模板 将#tp1模板覆盖并放到div#app里面*/
    var mvm = new Vue({
        el: '#app',
        template:'#tp1'
    })
</script>
```

#### 2.1.2 数据

* 只有初始化时传入的对象才是响应式的,即初始化之后vm.$data.b='2'对{{b}}无效.

* 在组件中,data的值必须为function且return原始对象,否则多个组件公用data.

* data和props中不能重名,否则异常.

```
<!-- 会报错,b需要在new Vue()之前实例化 -->
<script src="../js/vue.js"></script>

<body>
	<div id="app">
		<p>{{a}}</p>
		<p>{{b}}</p>
	</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data:{
            a: 1
        }
    })
</script>   
```

```
<script src="../js/vue.js"></script>

<body>
	<div id="app">
		<my-component title="组件" content="组件内容"></my-component>
	</div>
</body>

<script>
    var MyComponent = Vue.component('my-component', {
        props:['title', 'content'],
        data:function(){
            return {
                desc:'123'
            }
        },
        template:'<div>\
            <h1>{{title}}</h1>\
            <h1>{{content}}</h1>\
            <h1>{{desc}}</h1>\
            </div>'
    })
    var vm = new Vue({
        el: '#app',
        component:'MyComponent'
    })
</script>
```

#### 2.1.3 方法

* methods对象定义方法,v-on监听DOM事件.

```
<script src="../js/vue.js"></script>

<body>
    <div id="app">
        <button v-on:click="alert">click</button>
    </div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data:{
            a:'click方法'
        },
        methods:{
            alert:function(){
                alert(this.a)
            }
        }
    })
</script>
```

#### 2.1.4 生命周期(钩子)

| 生命周期方法  | 说明                                                         |
| :------------ | :----------------------------------------------------------- |
| beforeCreate  | 实例开始初始化时同步调用。数据观测、事件等都未初始化。       |
| created       | 实例创建之后调用。已完成数据绑定、事件方法，但DOM尚未编译。  |
| beforeMount   | 在mounted之前调用。                                          |
| mounted       | 在编译时调用。此时所有指令已失效，数据变化能触发DOM更新，但不能保证$el已经插入文档。 |
| beforeDestory | 开始销毁实例时调用，此刻实例仍然有效。                       |
| destoryed     | 实例销毁之后调用。此时所有指令解绑，子实例也被销毁。         |
| beforeUpdate  | 在实例挂载之后，再次更新实例(例如更新data)时会调用该方法，此时尚未更新DOM。 |
| updated       | 在实例挂载之后，再次更新实例并完成DOM结构后调用。            |
| activated     | 需要配合动态组件keep-live属性使用。在动态组件初始化渲染的过程中调用该方法。 |
| deactivated   | 需要配合动态组件keep-live属性使用。在动态组件移出的过程中调用该方法。 |

