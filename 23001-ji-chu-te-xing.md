## 2、基础特性

### 2.1 实例及选项

#### 2.1.1 模板

* Vue.js实例必须只有一个根div，即组件模板的内容必须封装在一个div里面。

```html
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



