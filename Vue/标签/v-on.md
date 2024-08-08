`v-on` 是 Vue.js 中的一个指令，用于监听 DOM 事件并绑定到 Vue 实例的方法。它是 Vue 事件处理的核心部分，允许你在组件中处理用户交互和事件。

### 基本用法

`v-on` 指令的基本语法如下：

```html
<button v-on:click="handleClick">Click me</button>
```

在上面的示例中，当按钮被点击时，会调用 Vue 实例中名为 `handleClick` 的方法。

### 简写语法

`v-on` 还有一个简写语法，使用 `@` 符号代替 `v-on`：

```html
<button @click="handleClick">Click me</button>
```

这种简写方式与 `v-on:click` 是等价的。

### 事件处理方法

在 Vue 实例中，你可以定义事件处理方法来响应用户操作。例如：

```javascript
new Vue({
  el: '#app',
  methods: {
    handleClick() {
      alert('Button clicked!');
    }
  }
});
```

### 事件对象

事件处理方法可以接收事件对象作为参数，这使得你可以访问事件的详细信息：

```html
<button @click="handleClick($event)">Click me</button>
```

```javascript
new Vue({
  el: '#app',
  methods: {
    handleClick(event) {
      console.log(event); // 打印事件对象
    }
  }
});
```

### 修饰符

`v-on` 指令支持事件修饰符，用于处理常见的事件管理需求。常用的修饰符包括：

- **`.stop`**: 调用 `event.stopPropagation()`，阻止事件冒泡。

  ```html
  <button @click.stop="handleClick">Click me</button>
  ```

- **`.prevent`**: 调用 `event.preventDefault()`，阻止默认行为。

  ```html
  <form @submit.prevent="handleSubmit">Submit</form>
  ```

- **`.capture`**: 使事件在捕获阶段触发，而不是在冒泡阶段触发。

  ```html
  <button @click.capture="handleClick">Click me</button>
  ```

- **`.once`**: 事件只会触发一次，触发后移除监听器。

  ```html
  <button @click.once="handleClick">Click me</button>
  ```

- **`.passive`**: 将事件处理函数标记为被动，优化性能，防止调用 `preventDefault()`。

  ```html
  <button @click.passive="handleClick">Click me</button>
  ```

### 事件修饰符组合

你可以组合使用多个修饰符：

```html
<button @click.stop.prevent="handleClick">Click me</button>
```

### 事件绑定到子组件

你可以将事件绑定到子组件的自定义事件：

```html
<my-component @custom-event="handleCustomEvent"></my-component>
```

### 动态事件处理

你可以使用 JavaScript 表达式来动态绑定事件处理方法：

```html
<button @click="handleClick(methodName)">Click me</button>
```

```javascript
methods: {
  handleClick(method) {
    this[method]();
  },
  methodName() {
    console.log('Method called');
  }
}
```

### 处理键盘事件

`v-on` 支持处理键盘事件，可以通过修饰符来限制响应特定键：

- **`.enter`**: 监听 `Enter` 键。
- **`.esc`**: 监听 `Escape` 键。
- **`.tab`**: 监听 `Tab` 键。
- **`.delete`**: 监听 `Delete` 键。
- **`.up`**: 监听 `Up` 键。
- **`.down`**: 监听 `Down` 键。
- **`.left`**: 监听 `Left` 键。
- **`.right`**: 监听 `Right` 键。

例如：

```html
<input @keyup.enter="submitForm">
```

### 总结

`v-on` 指令是 Vue.js 事件处理的核心工具，通过它可以方便地绑定和处理 DOM 事件。使用 `v-on`，你可以监听用户的各种操作，并做出响应，极大地增强了 Vue.js 应用的互动性。