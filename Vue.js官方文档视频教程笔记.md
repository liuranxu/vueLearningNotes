## 序言 Vue.js介绍

#### Vue.js 优点

1. 体积小33K
2. 运行效率高，基于虚拟DOM
3. 双向数据绑定
4. 生态丰富、学习成本低

## 第1节 安装与部署

#### 使用<script>标签直接引入Vue.js

引入本地vue.js文件语法：

```html
<script src="./vue.script"></script>
```

通过CDN引入vue.js文件语法：

```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

当下载并使用<script>标签引入Vue.js后，Vue会被注册为一个全局变量，在代码文件中使用vue()函数创建Vue实例，从而实现vue操作。

## 第2节 创建第一个vue应用

#### 声明式渲染

Vue.js的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进DOM的系统

声明式渲染语法1 -- 文本插值：

```html
<div id="app">
	{{ message }}
</div>
```

```javascript
var app = new Vue({
	el: '#app',
	data: {
		message: 'hello Vue!'
	}
})
```

当在控制台中修改app.message的值时，页面渲染出的值改变

声明式渲染语法2 -- 绑定元素attribute

```html
<div id = "app-2">
    <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
    </span>
</div>
```

```javascript
var app2 = new Vue({
	el: '#app-2',
	data: {
		message: '页面加载于' + new Data().toLocaleString()
	}
})
```

## 第3节 数据与方法

#### new Vue()

每个Vue应用都是通过用Vue函数创建一个新的Vue实例开始的，通常使用一个变量接收Vue函数被new之后的结果，也就是作为Vue的一个对象，通常使用vm(ViewModel视图模型的缩写)来代表Vue的一个实例，该过程语法如下：

```javascript
var vm = new Vue({

})
```

#### data: data

当一个Vue实例被创建时，它将data对象中的所有属性加入到Vue的响应式系统中。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值，示例如下：

```html
<div id="app">
</div>
```

```javascript
var data = { a : 1 };
var vm = new Vue({
    el: "#app",
    data: data
});
```

```javascript
data.a = "hi";
```

```javascript
vm.a = "hi, again";
```

以上改变data.a的值和vm.a的值效果相同，另外如果希望显示变量，则需要在new Vue()中就对该变量进行声明

#### Object.freeze()

Vue提供Object.freeze()方法，阻止修改现有的属性，同时响应系统中无法追踪变化，实例如下：

```html
<div id = "app">
    <p>{{ foo}}</p>
    <button v-on:click="foo = 'baz'">Change it</button>
</div>
```

```javascript
var obj = {
	foo: 'bar'
}

Object.freeze(obj)

new Vue({
    el: '#app',
    data: obj
})
```

以上语句使用freeze()方法让obj对象不再进行响应式

#### Vue实例属性和方法

Vue实例的实例属性和方法都有前缀$，以便与用户定义的属性区分开来，实例如下：

```javascript
var data = { a: 1}
var vm = new Vue({
	el: '#example',
	data: data
})

console.log(vm.$data === data); //true
console.log(vm.$el = document.getElementById('example')); //true

//$watch是一个实例方法，注意该方法是放在data.2 = 2;语句之前
vm.$watch('a', function (newValue, oldValue) {
    console.log("newValue: ", newValue); //2
    console.log("oldValue: ", oldValue); //1
})

data.a = 2;
```

注意：$watch()方法是放置于数据修改语句，即data.2 = 2的前面

## 第4节 生命周期

#### Vue实例的初始化过程

Vue实例在创建时需要经过一系列的初始化过程，例如设置数据监听、编译模板、将实例挂在到DOM并在数据变化时更新DOM等。

#### 命周期钩子

在Vue实例初始化过程中会运行一些叫做生命周期钩子的函数，可以借此在不同阶段添加需要的代码，生命周期钩子使用方法如下：

```javascript
new Vue({
	data: {
		a: 1
	},
	created: function() {
		//'this'指向vm实例
		console.log('a is: ' + this.a);
	}
})
```

#### 生期周期钩子API

在Vue官网>学习>API>[选项/生命周期钩子章节]([https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90](https://cn.vuejs.org/v2/api/#选项-生命周期钩子))中可以查看所有生命周期钩子API

#### 生命周期钩子用法

生命周期钩子代码需要写在使用Vue函数创建的对象内，以对象属性的方式声明，该属性为一个函数，在不同生命周期阶段系统将自动调用这些函数，以beforeCreate、Created、beforeMount、mounted、beforeUpdate、updated三组常用生命周期函数为例，实例如下：

```html
<div id="app">
	{{msg}}
</div>
```

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        msg: "hi vue",
    },
    beforeCreate: function(){
        console.log('beforeCreate');
    },
    created: function(){
    	console.log('created');
	},
	beforeMount: function(){
        console.log('beforeMount');
    },
    mounted: function(){
        console.log('mounted');
    }
})；

setTimeout(function(){
    vm.msg = "change......";
}, 3000);
```

## 第五节 模板语法-插值

#### 模板语法

Vue.js使用了基于HTML的模板语法。允许开发者声明式地将DOM绑定至底层Vue实例的数据。

#### 文本插值

数据绑定最常见的形式就是使用“Mustache”语法（双大括号）的文本插值，实例如下：

```html
<span>Message: {{ msg }}</span>
```

```javascript
var vm = new Vue({
	el: "span",
	data: {
		msg: "模板语法-插值"
	}
})
```

使用v-once指令可以实现一次性插值，即当数据改变时，插值处的内容不会更新。

```html
<span v-once>使用v-once指令实现一次性插值：{{ msg }}</span>
```

```javascript
var vm = new Vue({
	el: "span,
	data: {
		msg: "使用v-once该插值处的内容无法改变",
	}
})
```

此时使用如下代码修改msg的值，插值处的内容不变。

```javascript
vm.msg = "test";
```

#### 原始HTML

双大括号{{}}会将数据解释为普通文本，而非HTML代码。为了输出真正的HTML，需要使用v-html指令，代码如下：

```html
<div>
	<p>Using mustaches: {{ rawHtml }}</p>
	<p>Using v-html directive: <span v-html="rawHtml"></span></p>
</div>
```

```javascript
var vm3 = new Vue({
	el: "div",
	data: {
		rawHtml: '<span style="color:red">This should be red.</span>',
	}
})
```



























