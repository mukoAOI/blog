在 Vue.js 中，标签主要分为两类：内置指令标签和组件标签。以下是一些常用的 Vue.js 标签和它们的用途：

### 内置指令标签

1. **`<template>`**: 用于定义模板，不能渲染到 DOM 中，但可以包含多个根元素。
   
2. **`<script>`**: 用于编写 Vue 组件的 JavaScript 代码。
   
3. **`<style>`**: 用于定义组件的样式。可以使用 `scoped` 属性来限制样式只应用于当前组件。

### 常用内置指令

- **`v-if`**: 条件渲染，根据表达式的值决定是否渲染元素。
- **`v-else-if`**: `v-if` 的后续条件分支。
- **`v-else`**: `v-if` 的 else 分支。
- **`v-for`**: 列表渲染，基于数组或对象生成元素列表。
- **`v-model`**: 双向绑定表单控件的值。
- **`v-bind`**: 动态绑定 HTML 属性。
- **`v-on`**: 监听 DOM 事件。

### 组件标签

Vue 组件可以使用自定义标签来创建和使用。例如：

1. **基础组件**：
   ```html
   <my-component></my-component>
   ```

2. **动态组件**：
   ```html
   <component :is="currentComponent"></component>
   ```

3. **异步组件**：
   ```html
   <async-component></async-component>
   ```

### 常见的 Vue 组件库标签

如果你使用了 Vue 组件库，比如 Element UI、Vuetify、Ant Design Vue 等，那么你还会看到这些库中定义的标签，例如：

- **Element UI**:
  - `<el-button></el-button>`
  - `<el-input></el-input>`
  
- **Vuetify**:
  - `<v-btn></v-btn>`
  - `<v-card></v-card>`
  
- **Ant Design Vue**:
  - `<a-button></a-button>`
  - `<a-input></a-input>`

### 自定义组件标签

你可以在 Vue 应用中创建自己的组件，并使用自定义标签。例如：

```html
<!-- 定义一个名为 MyComponent 的 Vue 组件 -->
<template>
  <div>
    <h1>Hello, World!</h1>
  </div>
</template>

<script>
export default {
  name: 'MyComponent'
}
</script>

<style scoped>
/* 组件样式 */
</style>
```

在父组件或根实例中使用：

```html
<my-component></my-component>
```

### 总结

Vue.js 的标签包括内置指令标签、Vue 组件标签以及从第三方组件库中引入的标签。了解这些标签的使用方法和特点，可以帮助你更高效地开发 Vue.js 应用。