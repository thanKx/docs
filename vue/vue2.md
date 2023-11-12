# 基础

## 事件

```js
@click.prevent=""  阻止默认
@click.stop="" 阻止冒泡
once 执行一次
capture 捕获模式
self 自身才能执行
passive 立即执行

可以连着写
@click.prevent.stop 
```

## 键盘监听
```js
@keydown 按下
@keyup 松开

# 按键的编码获取
function (event) {
	console.log(event.keyCode)
	console.log(event.key)
}
 
```
## 计算属性
```js

data: {
	firstName: "张三"
}
// 里面都是计算属性 不是方法
// 通过 插值法{{ name }} 使用
computed: {
	// 完整写法
	name: {
		get() {
			// 这里的this是vm(vue model)
			return this.firstName;	
		},
		// 里面一定要有属性改变才能驱使他改变
		set(value) {
			this.firstName = value
		}
	}
	// 简写（没有set方法的时候）不是方法，是计算属性只是简单写法
	name() {
		return this.firstName
	}
}
```
## 监视属性
```js

watch：{
	info：{ 
		immediate: true, // 立刻改变 
		handler(oldValue,newValue){
		},
		deep: true // 监视多级所有属性的变化 vue的watch不能监测内部值的改变
	}
}
```

# 指令

- 自定义一个v-big指令，绑定的值放大10倍
- 自定义一个v-fbind，获取值绑定值，且获取焦点

```javascript
new vue({
	directives:{
		// big 什么时候掉用？1 指令与元素绑定成功时。2 指定所以在的模版重新解析时。
		big(element,binding){
			element.innerText = binding.value * 10
		}
		fbind:{
			// 绑定时
			bind(element,binding) {
				element.value = binding.value
			},
			// 插入时
			inserted(element,binding) {
				element.foucus
			},
			// 更新时
			update(element,binding) {
				element.value = binding.value
				element.foucus
			}
		}
	}
})

```

# 生命周期

## 流程图

![vue生命周期](./vue生命周期.png)

# 组件

简称 VC

```javaScript
const school = vue.extend({
	name: "school",
	template: "<div>{{name}}</div>",
	data(){
		return{
			name: "学校名称"
		}
	}
})

const hello = vue.extend({
	name: "school",
	template: "<div>{{name}}</div>",
	data(){
		return{
			name: "学校名称"
		}
	}
})

// VM
new vue({
	components: {}
})
```

VueComponent 每次都返回一个全新的VueComponent

VM 管理的VC

VC 里面的this都是指自己 （VC）
VM 里面的this都是指自己（VM）

# 内置关系

这是js基础

`VueComponent.prototype.__proto__ === Vue.protptype `

```javascript
function Demo(){
	this.a = 1
	this.b = 2
}

const d = new Demo()

console.log(Demo.prototype) // 显式原型属性
console.log(d.__proto__) // 隐式原型属性

console.log(Demo.prototype === d.__proto__) // true

// 程序猿通过显式原型属性操作原型对象，追加一个x属性，值为99
Demo.prototype.x = 99

console.log()

```



# 单文件组件

## 简介

`xxx.vue` 需要经过加工，变成js,html,css

## 方式

- webpack
- 脚手架

## 写法

School.vue

```vue
<template>
	<div>
		
	</div>
</template>

<script>
</script>

<style>
</style>
```

main.js

```javascript
import Vue form 'Vue' // 这个是一个运行时的Vue，不存在模版解析器
import App form './App.vue'

new Vue({
	render(createElement){
		return createElement(App)
	}
	render: createElement => createElement(App)
	render: h => h(App)
	components: {App}
})
```

index.html

```html
<html>
	<body>
		<script type="module" src="/src/main.ts"></script>
	</body>
</html>
```

# props

> 让组件传入外部的值

``` javascript
	// 简单写法
	props: ["name","sex","age"]

	// 接受限制列席
	props: {
		name: String,
		sex: String,
		age: Number
	}
	
	// 默认值+必要
	props: {
    name: {
      type: String,
      required: true
      default: "张三"
    }
  }

	// props的值不能被修改
	// 如果要修改，可以放到data里面
	data() {
    return {
      myAge: this.age
    }
  }
	
```



# mixin

67课时

