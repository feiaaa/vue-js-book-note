## 4、过滤器

### 4.1 过滤器注册

Vue.js提供了全局方法Vue.filter()注册一个自定义过滤器，接受过滤器ID和过滤器函数两个参数。

使用函数的形式来标记参数 date | date('yyyy-MM-dd')

2.0中过滤器只能在{{}}标签中使用

```javascript
Vue.filter('date', function(value){
    if(!value instanceof Date) return value;
    return value.toLocalDateString();
})

<div>{{ date | date }}</div>
```

### 4.2 双向过滤器

在改变视图中的值之后，写回data绑定属性。

```javascript
// 处理价格
<input type="text" v-model="price | cents"/>

Vue.filter('cents', {
    read: function(value){
        return (value/100).toFixed(2);
    },
    write: function(value){
        return value*100;
    }
})
```

### 4.3 动态参数

支持在Vue()中定义的参数，不需要用引号括起来。