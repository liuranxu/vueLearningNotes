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

当下载并使用<script>标签引入Vue.js后，Vue会被注册为一个全局变量，在代码文件中使用vue()函数进入vue操作。

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

**当在控制台中修改app.message得值时，页面渲染出的值改变**

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

