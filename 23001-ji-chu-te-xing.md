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

```
<script>
    var vm = new Vue({
        el: '#app',
        data:{
            a:1
        },
        created: function(){
            console.log('created')
        }
    })
</script>
```

### 2.2 数据绑定

#### 2.2.1 数据绑定语法

```
var vm = new Vue({
	el: '#app',
    data: {
        id: 1,
        index: 0,
        name: 'Vue',
        avatar: 'http://...',
        count: [1, 2, 3, 4, 5],
        names: ['Vue1.0', 'Vue2.0'],
        items: [
            { name: 'Vue1.0', version: '1.0' },
            { name: 'Vue2.0', version: '2.0' }
        ]
	}
})
```

**1. 文本插值**  

- 最基础的形式是使用`{{ }}`  
- 更改vue实例值不变 `<span v-once="name">{{name}}</span>`  

**2. HTML属性**  

- `<div v-bind:id="'id-'+id"></div>`或简写`<div :id="'id-'+id"></div>`  

**3. 绑定表达式**  

- 表达式可以由一个简单的JavaScript表达式和可选的一个或多个过滤器构成  

```
{{ index+1 }}					//1
{{ index==0 ? 'a' : 'b' }}		//a
{{ name.split('').join('|') }}	//V|u|e
```

**4. 过滤器**  

- Vue.js允许在表达式后添加可选的过滤器，以管道符`|`指示  
- 内置10个过滤器  


| 过滤器     | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| capitalize | 字符串首字母大写                                             |
| uppercase  | 字符串转大写                                                 |
| lowercase  | 字符串转小写                                                 |
| currency   | 数字转化成货币符号，并自动添加数字分节号                     |
| pluralize  | 字符串复数化                                                 |
| json       | 将json对象数据输出成符合json格式的字符串                     |
| debounce   | 调用函数n毫秒后才会执行该动作，若在这之内重新调用，则重新计算时间 |
| limitBy    | 显示数组部分数据                                             |
| filterBy   | 过滤，可自定义过滤函数                                       |
| orderBy    | 排序，可自定义排序函数                                       |


```
{{ name | uppercase }}				//将name的值转换成大写
{{ name | filterA | filterB }}		//支持多个过滤器
{{ name | filterA arg1 arg2 }}		//将name的值，arg1，arg2传入过滤器A中
{{ name.split('') | limitBy 3 1 }}	//->u,e
```

**5. 指令**  

可以理解为，当表达式的值发生变化时，会出发DOM行为。指令一般带有`v-`前缀。

①参数，`v-bind:`可缩写为`:`。`<img v-bind:src="avatar"/>`同`<img src="{{avatar}}"/>`  
②修饰符，以`.`开始的特俗后缀。`<button v-on:ckick.stop="doClick"></button>`

#### 2.2.2 计算属性

**1. 基础例子**

```
<script src="../js/vue.js"></script>

<body>
    <div id="app">
        <input type="text" v-model="firstName">
        <p>{{ fullName }}</p>
    </div>
</body>

<script>
// 对firstName和lastName的修改始终会影响fullName
var vm = new Vue({
	el: '#app',
	data:{
		firstName: 'Hanson',
		lastName:'Yan'
	},
	computed: {
		fullName: function(){
			// this指向vm实例
			return this.firstName+' '+this.lastName
		}        
    }
})
</script>
```

**2. Setter**

可以将vm.cents设置为后端数据，前端计算price  

```
<script src="../js/vue.js"></script>

<body>
    <div id="app">
        <input type="text" v-model="price">
        <p>{{ cents }}</p>
    </div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            cents: 100
        },
        computed: {
            price: {
                set: function (newValue) {
                    this.cents = newValue * 100;
                },
                get: function () {
                    return (this.cents / 100).toFixed(2);
                }
            }
        }
    })
</script>
```

#### 2.2.3 表单控件

```
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            message: '',
            gender: '',
            checked: '',
            multiChecked: [],
            selected: '',
            multiSelected: []
        }
    })
</script>
```

**1. Text-输入框**

```
<input type="text" v-model="message"/>
<span>您输入的是：{{ message }}</span>
```

**2. Radio-单选框**

```
<label><input type="radio" value="male" v-model="gender">男</label>
<label><input type="radio" value="female" v-model="gender">女</label>
<p>{{ gender }}</p>
```

**3. Checkbox-单勾选/多勾选**

```
<!-- 单选 - 值为boolean类型，input的值并不影响v-model的值 -->
<input type="checkbox" v-model="checked"/>
<span>已选中：{{ checked }}</span>

<!-- 多选 - v-model使用相同的属性名称，且为数组 -->
<label><input type="checkbox" value="1" v-model="multiChecked">1</label>
<label><input type="checkbox" value="2" v-model="multiChecked">2</label>
<label><input type="checkbox" value="3" v-model="multiChecked">3</label>
<p>多选结果：{{ multiChecked.join('|') }}</p>
```

**4. Select-下拉单选/多选**

```
<!-- 单选 -->
<select v-model="selected">
	<option selected>A</option>
	<option>B</option>
	<option>C</option>
</select>
<span>选中：{{ selected }}</span>

<!-- 多选 -->
<select v-model="multiChecked" multiple>
	<option selected>A</option>
	<option>B</option>
	<option>C</option>
</select>
<span>选中：{{ multiChecked.join('|') }}</span>
```

**5. 绑定value**

> 表单空间的值同样可以绑定在Vue实例的动态属性上，用`v-bind`实现。

1. Checkbox  

`<input type="checkbox" v-model="checked" v-bind:true-value="a" v-bind:false-value="b">`  
选中：`vm.checked==vm.a --> true`  
未选：`vm.checked==vm.b --> true`  

2. Radio  

`<input type="radio" v-model="checked" v-bind:value="a">`  
选中：`vm.checked==vm.a --> true`  

3. Select Options  

```
<select v-model="selected">
	<!-- 对象字面量 -->
	<option v-bind:value="{number: 123}">123</option>
</select>
{{selected.number}}
```

**6. 参数特性**

| 参数     | 特性                                                         |
| -------- | ------------------------------------------------------------ |
| lazy     | `<input v-model.lazy="query"/>` v-model默认同步修改，lazy修改之后再同步 |
| number   | `<input v-model.number="age"/>`自动处理number，若转换后结果为NaN则返回原值 |
| debounce | `<input v-model="query" debounce/>` 单位为ms，单位时间内仅更新一次。 |

#### 2.2.4 Class与Style绑定

**1. Class绑定**

> 可以绑定对象或数组

①对象语法：`v-bind:class`接收参数是一个对象，可以与普通class属性共存。
```
<div class="tab" v-bind:class="{'active': active, 'unactive': !active}"></div>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            active: true
        }
    })
</script>

渲染结果：<div class="tab active></div>
```

②数组语法：`v-bind:class` 也可接受数组为参数。  

```
<div class="tab" v-bind:class="[classA, classB]"></div>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            classA: 'class-a',
            classA: 'class-a'
        }
    })
</script>

渲染结果： <div class="class-a class-b"></div>
也可以使用三目表达式：<div v-bind:class="[classA, isB ? classB:]"></div>
```

**2. 内联样式绑定**

> 同样具有对象和数组两种形式

①对象语法：直接绑定符合样式格式的对象。

```
<div v-bind:style="alertStyle"></div>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            alertStyle: {
                color: 'red',
                fontSize: '20px'
            }
        }
    })
</script>

也可以绑定单个属性或直接使用字符串：`<div v-bind:style="{fontSize: alertStyle.fontSize, color:'red'}"></div>`
```

②数组语法：`v-bind:style` 允许将多个样式绑定到统一元素上。

```
<div v-bind:style="[styleObjectA, styleOjectB]"></div>
```

**3. 自动添加前缀**

在使用`transform` 这类属性时，`v-bind:style` 会根据需要自动添加厂商前缀。

### 2.3 模板渲染

#### 2.3.1 前后端渲染对比

前端渲染优点：

- 业务分离
- 计算量转移

后端渲染优点：

- 搜索引擎友好
- 首页加载时间短

#### 2.3.2 条件渲染

> Vue.js提供 `v-if` `v-show` `v-else` `v-for` 这几个指令来说明模板和数据间的逻辑关系。

**1. v-if / v-else**

`v-else` 必须紧跟`v-if` 否则不起作用

```
data.yes = true才显示if内容
<div v-if="yes">yes</div>
<div v-else="no">yes</div>
```

**2. v-show**

`v-show` 可与`v-if` 一样搭配`v-else` 使用；

`v-show` 与`v-if` 不同的是，`v-show` 的元素会保持在DOM，只是display的显示方式改变。

**3. v-show vs v-if**

`v-if` 切换时消耗较高，`v-show` 初始渲染时消耗较高。

#### 2.3.3 列表渲染

`v-for` 可接受数组、对象、数字。

```
<ul>
    <!-- item为数据项，index为索引 -->
    <li v-for="(item, index) in items">
    	<h3>{{ item.title }}</h3>
        <p>{{ item.description }}</p>
    </li>
</ul>

var vm = new Vue({
	el: '#app',
	data: {
		items: [
			{title: 'title-1', description: 'description-1'},
			{title: 'title-2', description: 'description-2'},
			{title: 'title-3', description: 'description-3'}
		]
	}
})

也可遍历对象：`<li v-for="(key, value) in objectDemo"></li>`
重复n次：`<li v-for="n in 5"></li>`
```

#### 2.3.4 template标签用法

可以将指令作用在template上，且最后不会渲染。

### 2.4 事件绑定与监听

> Vue.js提供 `v-on` 来绑定事件。

#### 2.4.1 方法及内联语句处理器

`<button v-on:click="say">Say</button>` 可简写成 `<button @click='say'>Say</button>` 。say是Vue实例中methos里面自定义的一个方法。

```
// 1.内联JavaScript语句，仅限一条语句
<button v-on:click="sayFrom('哈哈哈哈')"></button>

// 2.获取原生DOM事件对象
<button v-on:click="showEvent">Event</button>
<button v-on:click="showEvent($event)">showEvent</button>
<button v-on:click="showEvent()">showEvent</button>	//这样写获取不到
var vm = new Vue({
	el: '#app',
	methods: {
		showEvent: function(ecent){
			console.log(event);
		}
	}
})

// 3.可以绑定多个相同事件函数，按顺序执行
<div v-on:click="sayFrom('first')" v-on:click="sayFrom('second')"></div>
```

#### 2.4.2 修饰符

主要修饰符：

- .stop 等同于调用event.stopPropagation()，停止之前的事件
- .prevent 等同于调用event.preventDefault()
- .capture 使用capture模式添加事件监听器
- .self 只当事件是从监听元素本身触发时才触发回调，例如用在div上点击里面的元素不会触发。

使用方式：

`<a v-on:click.stop='doThis'></a>` 

`form v-on:submit.prevent="onSubmit"></form>` //阻止表单默认提交事件

`<form v-on:submit.stop.prevent="onSubmit"></form>` //阻止默认提交事件且阻止冒泡

`<form v-on:submit.stop.prevent></form>` //也可只有修饰符并不绑定事件

### 2.5 Vue.extend()

> 生成可被重复使用的组件，创建基础Vue构造器的“子类”，参数options对象和直接声明VUE实例参数对象基本一致。

```
var Child = Vue.extend({
    template: '#child',
    // 不同的是，el和data项需要通过函数返回值赋值，避免多个组件实例共用一个数据
    data: function(){
        return {
            ...
        }
    }
})

Vue.component('child', Child)	//全局注册组件
<child></child>	//子组件在其他组件内的调用方式
```
