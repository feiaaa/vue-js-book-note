## 3、指令

### 3.1 内置指令

`v-bind` `v-model` `v-if/v-else/v-show` `v-for` `v-on` `v-text` `v-HTML` `v-el` `v-ref` `v-pre` `v-cloak` `v-once` 

`<div v-for="item in items" v-bind:key="item.id"></div>`

#### 3.1.1 v-bind的三种修饰符
.once ：【2.0废除】用于组件 props 属性，但进行的是单次绑定。（将绑定数据传递给子组件后，子组件单独维护这份数据，和父组件的数据
再无关系，父组件的数据发生变化也不会影响子组件中的数据。）
```
<my-child :parent.once='parent'></my-child>
```
.sync ：【2.0废除】用于组件 props 属性，进行双向绑定，即父组件绑定传递给子组件的值，无论在哪个组件中对其进行了修改，其他组件中的这个值也会随之更新。
```
<my-child :parent.sync='parent'></my-child>。
```
.camel ：将绑定的特性名字转回驼峰命名（HTML 属性不区分大小写）。只能用于普通 HTML 属性的绑定，通常会用于svg 标签下的属性 
```
<svg width='400' height='300' :view-box.camel='viewBox'></svg>

```
输出结果即为
```
 <svg width="400" height="300" viewBox="….."></svg>
```
在2.0情况下，只能单向传值，如果子要改父，靠事件机制。

### 3.2 自定义基础指令

#### 3.2.1 指令的注册

可以通过Vue.directive(id, definition)方法注册一个全局自定义指令。

```javascript
Vue.directive('global-directive', definition)	//注册指令

<div v-global-directive></div>	//使用指令
```

也可以通过在组件的directives选项注册一个局部的自定义指令。

```javascript
var comp = Vue.extend({
    directives: {
        'localDirective':{}	//可以采用驼峰式命名
    }
})

在组件内部使用v-local-directive
```

#### 3.2.2 指令的定义对象

在注册指令的同时，可以传入definition定义对象，对指令赋予功能。这个定义对象主要包括三个钩子函数：bind、update和unbind。

- bind：只被调用一次，在指令第一次绑定到元素上时起作用。
- update：指令在bind之后以初始值为参数进行第一次调用，之后每次当绑定值发生变化时调用，update接收到的参数为newValue和oldValue。
- unbind：指令从元素上解绑时调用，只调用一次。

这三个函数都是可选的，但注册一个空指令肯定没有意义。

```javascript
<div v-if="isExist" v-my-directive="param"></div>

Vue.directive('my-directive', {
    bind: function(){
        console.log('bind', arguments);
    },
    update: function(newValue, oldValue){
        console.log('update', newValue, oldValue);
    },
    unbind: function(){
        consloe.log('unbind', arguments);
    }
})

var vm = new Vue({
    el: '#app',
    data: {
        param: 'first',
        isExist: true
    }
})
```

#### 3.2.3 指令实例属性

一个指令会有如下属性：

el：指令绑定的元素；

vm：指令的上下文ViewModel；

expression：指令的表达式，不包括参数和过滤器；

arg：指令的参数；

name：指令的名字，不包括v-前缀；

modifiers：一个对象，包含指令的修饰符；

descriptor：一个对象，包含指令的解析结果。

#### 3.2.4 元素指令(2.0已取消)

普通指令需要绑定在具体某个DOM元素上，元素指令可以单独存在，使用方式类似组件，但是本省的实例属性和钩子函数是和指令一致的。

元素指令也有局部和全局注册两种方式。

```javascript
Vue.elementDirective('my-ele-directive')	//全局注册方式

var Comp = Vue.extend({		//局部注册，仅限该组件内使用
    elementDirectives: {
        'eleDirective' : {
            
        }
    }
})
Vue.component('comp', Comp)
```

### 3.3 指令的高级选项

#### 3.3.1 params

#### 3.3.2 deep

#### 3.3.3 twoWay 【vue2被废弃】
在自定义指令中，如果需要向 Vue 实例写回数据，就需要在定义对象中使用 twoWay:true，
这样可以在指令中使用 this.set(value) 来写回数据

[如何在Vue2中实现组件props双向绑定](https://www.cnblogs.com/xxcanghai/p/6124699.html)
Vue 2.x相比较Vue 1.x而言，升级变化除了实现了Virtual-Dom以外，给使用者最大不适就是移除的组件的props的双向绑定功能。
以往在Vue1.x中利用props的twoWay和.sync绑定修饰符就可以实现props的双向绑定功能，但是在Vue2中彻底废弃了此功能，如果需要双向绑定需要自己来实现。

#### 3.3.4 acceptStatement

#### 3.3.5 terminal

#### 3.3.6 priority

### 3.4 指令在2.0中的变化

#### 3.4.1 新的钩子函数

> 钩子函数增加了componentUpdated，当整个组件都完成了update状态后即所有DOM都更新后调用该钩子函数，无论指令接受的参数是否发生变化。

#### 3.4.2 钩子函数实例和参数变化

this并不能指向指令的相关属性，均通过参数的形式传递给钩子函数。

#### 3.4.3 update函数触发变化

①指令绑定bind函数执行后不直接调用update函数

②只要组件发生重绘，就会调用update函数。使用binding.value=binding.oldValue来判断过滤不必要更新。

#### 3.4.4 参数binding对象

钩子函数接受的参数binding不可更改，只能通过el直接修改DOM元素。

