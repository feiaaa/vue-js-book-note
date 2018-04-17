## 2、基础特性

### 2.1 实例及选项

#### 2.1.1 模板

* Vue.js实例必须只有一个根div，即组件模板的内容必须封装在一个div里面。

```
<div id="app">
    <!-- 内容会被模板覆盖 -->
    <p>123</p>
</div>

<script id="tp1" type="x-template">
    <div>
        <p>模板</p>
    </div>
</script>


/* 模板 将#tp1模板覆盖并放到div#app里面*/
var mvm = new Vue({
    el: '#app',
    template:'#tp1'
})
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





