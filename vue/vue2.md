# Vue.js 基础知识整理

## 事件处理

### 常用事件修饰符

```
javascriptCopy code
@click.prevent=""  // 阻止默认行为
@click.stop=""    // 阻止事件冒泡
@click.prevent.stop // 阻止默认行为并阻止冒泡
@click.once       // 事件只触发一次
@click.capture    // 使用事件捕获模式
@click.self       // 只有事件在该元素本身触发时才生效
@click.passive    // 提高滚动性能的修饰符
```

### 键盘事件监听

```
javascriptCopy code
@keydown // 按键被按下时触发
@keyup   // 按键被松开时触发

// 获取按键的编码
function(event) {
  console.log(event.keyCode); // 旧的按键编码方式
  console.log(event.key);     // 新的按键字符方式
}
```

## 计算属性

```
javascriptCopy code
data: {
  firstName: "张三"
},
computed: {
  // 完整写法
  name: {
    get() {
      return this.firstName; // this 指向 Vue 实例
    },
    set(value) {
      this.firstName = value;
    }
  },
  // 简写方式（没有 set 方法）
  name() {
    return this.firstName;
  }
}
```

## 监视属性

```
javascriptCopy code
watch: {
  info: { 
    immediate: true, // 立即触发
    handler(oldValue, newValue) {
      // 处理函数
    },
    deep: true // 深度监听
  }
}
```

## 自定义指令

### 示例

```
javascriptCopy code
new Vue({
  directives: {
    big(element, binding) {
      element.innerText = binding.value * 10;
    },
    fbind: {
      bind(element, binding) {
        element.value = binding.value;
      },
      inserted(element, binding) {
        element.focus();
      },
      update(element, binding) {
        element.value = binding.value;
        element.focus();
      }
    }
  }
})
```

## Vue 生命周期

### 生命周期流程图

![vue生命周期](./vue生命周期.png)

## 组件（VC）

### 示例

```
javascriptCopy code
const school = Vue.extend({
  name: "school",
  template: "<div>{{name}}</div>",
  data() {
    return {
      name: "学校名称"
    }
  }
});

const hello = Vue.extend({
  name: "hello",
  template: "<div>{{name}}</div>",
  data() {
    return {
      name: "问候语"
    }
  }
});

new Vue({
  components: { school, hello }
});
```

### 组件与实例

- `VueComponent` 每次返回一个全新的组件实例。
- `VM`（Vue 实例）管理 VC（Vue 组件）。
- `VC` 中的 `this` 指向自己。
- `VM` 中的 `this` 指向 Vue 实例自身。

## 内置关系

### JavaScript 原型链示例

```
javascriptCopy code
function Demo() {
  this.a = 1;
  this.b = 2;
}

const d = new Demo();

console.log(Demo.prototype); // 显式原型属性
console.log(d.__proto__);    // 隐式原型属性

console.log(Demo.prototype === d.__proto__); // true

Demo.prototype.x = 99; // 通过原型添加属性
```

## 单文件组件

### 概述

- `.vue` 文件需要转换成 JavaScript、HTML 和 CSS。
- 转换工具包括 Webpack 和 Vue CLI。

### 示例

```
School.vue
vueCopy code
<template>
  <div></div>
</template>

<script>
export default {
  name: "School"
  // 组件逻辑
};
</script>

<style>
/* 样式 */
</style>
main.js
javascriptCopy code
import Vue from 'Vue';
import App from './App.vue';

new Vue({
  render: h => h(App)
}).$mount('#app');
index.html
htmlCopy code
<!DOCTYPE html>
<html>
  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

## Props

### 用法

```
javascriptCopy code
props: {
  name: String,
  sex: String,
  age: Number
}
```

### 默认值和验证

```
javascriptCopy code
props: {
  name: {
    type: String,
    required: true,
    default: "张三"
  }
}
```

### 注意事项

- `props` 的值不应直接修改。
- 若要修改，可以先复制到 `data` 中。

## Mixin

### 用途

Mixin 用于在多个组件间共享功能
