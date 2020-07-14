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

#### 特性

使用v-bind指令实现HTML attribute模板语法

```html
<div id="red">
    <div v-bind:class="color">test</div>
</div>
```

```javascript
var vm = new Vue({
	el: "#red",
	data: {
		color: 'red'
	}
});
```

```css
.red {
	color: red;
}
```

#### 模板语法实现JavaScript表达式

实例如下：

```html
<p>{{ number + 1 }}</p>
<p>{{ ok ? 'YES' : 'No' }}</p>
<p>{{ message.split('').reverse().join('') }}</pp>
```

```javascript
var vm = new Vue({
	el: "p",
	data: {
		number ：10，
		ok : false,
        message: "vue"
	}
})
```

## 第六节 模板语法-指令

#### 指令

指令(directives)是带有v-前缀的特殊特性。指令特性的值预期是单个Javascript表达式(v-for例外)。

指令的职责是当表达式的值改变时，将其产生的连带影响，响应式地作用于DOM，实例如下：

```html
<p v-if="seen">现在你看到我了</p>
```

```javascript
var vm = new Vue({
	el: "#app",
	data: {
		seen : true
	}
})
```

#### 参数

一些指令能够接收一个“参数”，在指令名称之后以冒号表示，例如v-bind指令可以用于响应式的更新HTML attribute，实例如下：

```html
<a v-bind:href="url">...</a>
```

```javascript
var vm = new Vue({
	el: "#app",
	data: {
		url : "http://github.com"
	}
})
```

#### 修饰符

修饰符(modifier)是以半角句号.指明的特殊后缀，用于指出一个指令应该以特殊方式绑定，例如.prevent修饰符告诉v-on指令对于触发的事件调用event.preventDefault()，实例如下：

```html
<div v-on:click="click1">
	<div v-on:click.stop="click2">
		click me!
	</div>
</div>
```

```javascript
methods: {
	click1 : function() {
		console.log('click1');
	},
	click2 : function() {
		console.log('click2');
	}
}
```

## 第7节 class与style绑定

#### v-bind

在vue数据绑定中，可以使用v-bind指令操作元素的class列表和内联样式，只需要通过表达式计算出字符串结果即可。为应对字符串拼接麻烦且易错，在将v-bind用于class和style时，Vue.js做了专门的增强。表达式结果类型除了字符串之外还可以是对象或数组。

#### 使用对象绑定HTML Class

可以传给v-bind:class一个对象，以动态的切换class，实例如下：

```html
<div id="app">
	<div 
	class="font-size"
	v-bind:class="{ active: isActive , green : isGreen}"
	style="width:200px; height:200px; text-align:center; line-height:200px;">
	hi vue
	</div>    
</div>
```

```javascript
var vm = new Vue({
	el: "#app",
	data: {
		isActive : true,
         isGreem : true
	}
});
```

```css
.green {
    color: green;
}
.active {
    background: red;
}
```

#### 使用数组绑定HTML Class

同样也可以把一个数组传给v-bind:class，以应用一个class列表，实例如下：

```html
<div id="app">
	<div class = v-bind：class="[ isActive ? 'active' : '', isGreen ? 'green' : '']"
<div>
```

```javascript
var vm = new Vue({
	el: "#app",
	data: {
		isActive : false,
		isGreen : true
	}
});
```

```css
.active {
	background: red;
}
.green {
	color: green;
}
```

#### 使用对象绑定内联样式

使用一个Javascript对象v-bind:style绑定内联样式。CSS属性名可以使用驼峰式(camelCase)或者短横线分隔(Kebab-case，短横线分隔法的属性名需要用引号括起来)。

直接将内联样式绑定到一个样式对象通常更好，这会让模板更清晰。

实例如下：

```html
<div 
v-bind:style="{ color: color, fontSize: fontSize, background: isRed ? '#FF0000' : ''}"
>
	hi vue again
</div>
```

```javascript
var vm = new Vue({
	el: "#app",
	data: {
		color: 'yellow',
		fontSize: '50px',
        isRed : true
	}
})
```

## 第8节 条件渲染

#### v-if

使用v-if指令条件性地渲染一块内容。这块内容只会在指令的表达式返回truthy值的时候被渲染，实例如下：

```html
<div id="app">
    <h1 v-if="awesome">Vue is awesome!</h1>
	<h1 v-else>Oh no</h1>
</div>
```

```javascript
var vm = new Vue({
	el: "#app",
    data: {
        awesome : true
    }
})
```

#### v-else-if

v-else-if是指可以充当v-if的“else-if块”，可以连续使用，实例如下：

```html
<div id="app">
	<div v-if="type === 'A'">
		A
	</div>
    <div v-else-if="type === 'B'">
        B
    </div>
    <div v-else-if="type === 'C'">
        C
    </div>
    <div v-else>
        Not A/B/C
    </div>
</div>
```

#### v-show

带有v-show的元素始终会被渲染并保留在DOM中，v-show知识简单的切换元素的CSS属性display，当v-show中赋予变量值为false时，display为none。实例如下：

```html
<div id="#app3">
	<div v-show="ok">hello v-show!</div>
</div>
```

```javascript
var vm3 = new Vue({
	el: "#app3",
	data: {
		ok : true
	}
})
```

## 列表渲染

#### v-for基于数组渲染列表

可以用v-for指令基于一个数组渲染一个列表。v-for指令需要使用item in items形式的特殊语法，其中items时原数据数组，而item则是被迭代的数组元素的别名，可以使用index获取item的索引。实例如下：

```html
<ul id="exaample-1">
	<li v-for="item,index in items" v-bind:key="index">
		{{ index }}{{ item.message }}
	</li>
</ul>
```

```javascript
var example1 = new Vue({
	el: '#example-1',
	data: {
		items: [
			{ message: 'Foo' },
			{ message: 'Bar' }
		]
	}
})
```

#### v-for使用对象渲染数组

可以使用v-for来遍历一个对象的属性，可以使用key获取对象的键，实例如下：

```html
<ul id="v-for-object" class="demo">
	<li v-for="value,key in object" v-bind:key="key">
		{{key}}: {{ value }}
	</li>
</ul>
```

```javascript
var object1 = new Vue({
	el: "#v-for-object",
	data: {
		object: {
			title: 'How to do lists in Vue',
			author: 'Jane Doe',
			publishedAt: '2016-04-10'
		}
	}
})
```

## 第10节 事件绑定

#### v-on中直接添加代码

可以使用v-on指令监听DOM事件，并在触发时运行一些JavaScript代码，实例如下：

```html
<div id="example-1">
    <button v-on:click="counter += 1">Add 1</button>
    <p>The button above has been clicked {{ counter }} times.</p>
</div>
```

```javascript
var example1 = new Vue({
	el: '#example-1',
	data: {
		counter: 0
	}
})
```

#### v-on中添加方法名称

由于许多事件处理逻辑更加复杂，所以直接将JavaScript代码写在v-on指令中是不可行的。v-on可以接收一个需要调用的方法名称，同时在new Vue()中添加methods属性，在method属性中添加函数名和函数体。实例如下：

```html
<div id="example-1">
	<button v-on:dblclick="greet('abc', $event)">greet</button>
</div>
```

```javascript
var example1 = new Vue({
	el: '#example-1',
	data: {
		name : 'vue'
	}
	methods: {
		greet : function(str, e) {
			alert("hi");
			alert(this.name);
			alert(str);
			console.log(e);
		}
	}
})
```

## 第11节 表单输入绑定

#### v-model

可以使用v-model指令在表单<input>、<textarea>和<select>元素上创建双向数据绑定。它会根据控件类型选取正确的方法来更新元素。

v-model在内部为不同的输入元素使用不同的属性并抛出不同的事件：

* text和textarea元素使用value属性和input事件；
* checkbox和radio使用checked属性和change事件；
* select字段将value作为prop并将change作为事件。

示例如下：

```html
<div id="app">
        <div id="example-1">
            <input v-model="message" placeholder="edit me">
            <p>Message is: {{ message }}</p>
            <textarea v-model="message2" placeholder="add multiple lines"></textarea>
            <p style="white-space: pre-line;">{{ message2 }}</p>
            <br>
        </div>

        <div style="margin-top:20px;">
            <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
            <label for="jack">Jack</label>
            <input type="checkbox" id="john" value="John" v-model="checkedNames">
            <label for="john">John</label>
            <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
            <label for="mike">Mike</label>
            <br>
            <span>Checked names: {{ checkedNames }}</span>
        </div>

        <div style="margin-top:20px;">
            <input type="radio" id="one" value="One" v-model="picked">
            <label for="one">One</label>
            <br>
            <input type="radio" id="two" value="Two" v-model="picked">
            <label for="two">Two</label>
            <br>
            <span>Picked: {{ picked }}</span>
        </div>
        <button type="button" @click="submit">提交</button>
    </div>
```

```javascript
var vm = new Vue({
	el: "#app",
	data : {
		message : "this is a input",
		message2 : "this is a textarea",
		checkedNames : ['Jack', 'John'],
		picked : "Two"
	},
	methods: {
		submit : function() {
			console.log(this.message);
			var postObj = {
				msg1 : this.message,
				msg2 : this.message2,
				checkval : this.checkedNames
			};
			console.log(postObj);
		}
	}
});
```































