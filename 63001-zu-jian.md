## 6、组件

### 6.1 组件注册

创建组件构造器的方式

```javascript
var MyComponent = Vue.extend({...})
```

#### 6.1.1 全局注册

全局注册需要确保在根实例初始化前注册，这样才能使组件在任意实例中被使用。注册方式如下：

```javascript
Vue.component('my-component', MyComponent);
```

这条语句需要写在var vm = new Vue({...})之前，可以在模板中使用`<my-component></my-component>` 

#### 6.1.2 局部注册

局部注册限定了组件只能在被注册的组件中使用，而无法在其他组件中使用，注册方式如下：

```javascript
var Child = Vue.extend({
    template: '<p>子组件</p>'
});

var Parent = Vue.extend({
    template: '<div>\
    		<p>父组件</p>\
    		<my-child></my-child>\
    		</div>',
    components: {
        'my-child': Child
    }
}) 
```

#### 6.1.3 注册语法简写

对以上两种方式可以简写

```javascript
// 全局注册
Vue.component('my-component', {
    template: '<p>这是一个组件</p>'
})

// 局部注册
var Parent = Vue.extend({
    template: '<div>\
    	<p>父组件</p>\
    	<my-child></my-child>\
    	</div>',
    components: {
        'my-child': {
            template: '<p>我是子组件</p>'
        }
    }
})
```

### 6.2 组件选项

组件接受的选项大部分与Vue实例一样，主要区别在props用于接受父组件传递的参数。

#### 6.2.1 组件和Vue区别

在Vue中直接给data赋值：`data: {name: 'Vue'}` 

在组件中需要利用函数返回值：`data: function(){return {name: 'component'}}`

#### 6.2.2 组件Props

**注意：**props沟通子组件和父组件

子组件的模板和模块中无法直接调用父组件的数据，父组件通过props属性传值给子组件，子组件在接受数据时需要显示声明props。如下：

```javascript
Vue.component('my-child', {
    props: ['parent'],
    template: '<p>{{parent}} 来自父组件</p>'
})

<my-child parent="数据"></my-child>
```

**动态props**

默认单向绑定，即父组件影响子组件，子组件不影响父组件。

修饰符.sync和.once声明绑定为双向或单向。

```javascript
<div id="app">
        <input type="text" v-model="message"/>
        <my-component v-bind:message="message"></my-component>
    </div>

Vue.component('my-component', {
        props: ['message'],
        template: '<p>父组件消息：{{message}}</p>'
    })

var vm = new Vue({
        el: '#app',
        data: {
            message: 'default'
        }
    })
```

### 6.3 组件间通信

通过广播、派发、监听等形式进行跨组件的函数调用。

#### 6.3.1 直接访问

在组件实例中，Vue.js提供一下3个属性对其父子组件及根实例进行直接访问：

1. $parent：父组件实例
2. $children：包含所有子组件实例
3. $root：组件所在的根实例

三个属性都挂载在组件的this上，或导致父子组件紧密耦合，不提倡使用。尽量使用props。

#### 6.3.2 自定义事件监听

**1.events选项**

我们可以在初始化实例或注册子组件时，直接传选项events一个对象，如下：

```javascript
var vm = new Vue({
    el: '#app',
    data；{
    	todo: []
    },
    events: {
        'add': function(msg){
            this.todo.push(msg);
        }
    }
})
```

**2.$on方法**

我们也可以在某些特定情况或方法内采用$on方法来监听事件，如下：

```javascript
var vm = new Vue({
    el:'#app',
    data:{
        todo: []
    },
    methods: {
        begin: function(){
            this.$on('add', function(msg){
                this.todo.push(msg);
            })
        }
    }
})
```

#### 6.3.3 自定义事件触发机制

**1.$emit**

在实例本身触发事件

```javascript
events: {
    'add': function(msg){
        this.todo.push(msg);
    }
},
methods: {
    onClick: function(){
        this.$emit('add', '消息');	//可触发events中的add函数
    }
}
```

**2.$dispatch(废除)**

派发事件，事件沿着父链冒泡，并且在第一次触发回调之后自动停止冒泡，除非触发函数明确返回true，才继续向上冒泡。

```javascript
// 父组件
events: {
    'add': function(msg){
        this.todo.push(msg);	//return true 明确返回true之后，事件会继续向上冒泡
    }
}

//子组件
methods: {
    toParent: function(){
        this.$dispatch('add', '消息');	//子组件调用函数toParent，即可向上冒泡，触发父组件add
    }
}
```

**3.$broadcast(废除)**

广播事件，事件会向下传递给所有后代。

```javascript
// 父组件
methods: {
    toChild: function(){
        this.$broadcast('add', '消息');
    }
}

// 子组件
events: {
    'msg': function(msg){
        alert(msg);
    }
}
```

#### 6.3.4 子组件索引

除了之前的this.children之外，还可以给子组件绑定一个`v-ref` ,指定一个索引ID。

```html
<child-todo ref="first"></child-todo>
```

这样，在父组件中就可以通过this.children.first来获取。

### 6.4 内容分发

使用`<slot>` 元素为原始内容的插槽

#### 6.4.1 基础用法

```html
<body>
    <div id="app">
      // 使用包含slot标签属性的子组件
      <my-slot>
          // 属性slot值需要与子组件中slot的name值匹配
          <p slot="title">{{ title }}</p>
          <div slot="content">{{ content }}</div>
      </my-slot>
    </div>
    
</body>


<script>
    // 注册my-slot组件，包含<slot>标签，且设定唯一标识name
    Vue.component('my-slot', {
        template: '<div>\
            <div class="title">\
                <slot name="title"></slot>\
            </div>\
            <div class="content">\
                <slot name="content"></slot>\
            </div>\
        </div>'
    });

    var vm = new Vue({
        el: '#app',
        data: {
            title: '标题',
            content: '内容'
        }
    })
</script>
```

#### 6.4.2 编译作用域

`<slot>` name不能通过子组件赋值，否则无效。

### 6.5 动态组件

即多个组件使用同一个挂载点，根据条件切换不同的组件。一般用于路由控制或tab切换中。

#### 6.5.1 基础用法

```html
<body>
    <div id="app">
      // 相当于一级导航栏，点击可切换页面
      <ul>
          <li @click="currentView = 'home'">Home</li>
          <li @click="currentView = 'list'">List</li>
          <li @click="currentView = 'detail'">Detail</li>
      </ul>
        // 将渲染内容保存在内存，不用每次切换渲染
      <keep-alive>
      	<component :is="currentView"></component>
      </keep-alive>
    </div>
    
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            currentView: 'home'
        },
        components: {
            home: {
                template: '<div>Home</div>'
            },
            list: {
                template: '<div>List</div>'
            },
            detail: {
                template: '<div>Detail</div>'
            }
        }
    })
</script>
```

#### 6.5.2 activate钩子函数

比如上面中home组件修改为：

```javascript
home: {
	template: '<div>\
		<p>Home</p>\
		<ul>\
			<li v-for="item in items">{{item}}</li>\
         </ul>\
      </div>',
     data: function(){
          return {
            items: []
          }
     },
     activate: function(done){
          var that = this;
          // 模拟ajax异步请求数据
           setTimeout(function(){
               that.items = [1,2,3,4,5];
               done();
           }, 100000);
      }
}
```

### 6.6 Vue.js2.0废除event选项

所有自定义时间都需要通过`$emit` `$on` `$off` 函数来进行触发、监听和取消监听。

```html
<body>
    <div id="app">
      <comp-a></comp-a>
      <comp-b></comp-b>
    </div> 
</body>
<script>
    var bus = new Vue();
    var vm = new Vue({
        el: '#app',
        components: {
            compA: {
                template: '<div>\
                    <input type="text" v-model="name"/>\
                    <button @click="create">添加</button>\
                </div>',
                data: function(){
                    return {
                        name: ''
                    }
                },
                methods: {
                    create: function(){
                        bus.$emit('create', {name: this.name});
                        this.name = '';
                    }
                }
            },
            compB: {
                template: '<ul>\
                    <li v-for="item in items">{{item.name}}</li>\
                </ul>',
                data: function(){
                    return {
                        items: []
                    }
                },
                // 生命周期函数
                mounted() {
                    var that = this;
                    bus.$on('create', function(data){
                        that.items.push(data);
                    })
                }
            }
        }
    })
</script>
```