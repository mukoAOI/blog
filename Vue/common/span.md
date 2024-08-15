`<span>` 标签是一个 HTML 元素，用于在文档中创建一个内联容器。它本身不会带有任何特定的样式或布局效果，但它非常有用，可以与 CSS 和 JavaScript 一起使用来应用样式或处理 DOM 元素。

### 基本用法

```html
<span>This is a span element.</span>
```

### 用途

1. **样式应用**：
   `<span>` 标签通常用来在内联文本中应用样式。例如：
   ```html
   <p>这是一个 <span style="color: red;">红色的</span> 字段。</p>
   ```

2. **文本强调**：
   用于在文本中突出显示某些部分：
   ```html
   <p>这是一个 <span class="highlight">重要的</span> 词语。</p>
   ```
   对应的 CSS：
   ```css
   .highlight {
     background-color: yellow;
   }
   ```

3. **JavaScript 操作**：
   可以用 JavaScript 操作 `<span>` 元素：
   ```html
   <span id="mySpan">Click me!</span>
   
   <script>
     document.getElementById('mySpan').addEventListener('click', function() {
       alert('Span clicked!');
     });
   </script>
   ```

4. **内联元素**：
   由于 `<span>` 是内联元素，它不会换行，默认情况下它会与周围的内容在同一行显示：
   ```html
   <div>
     This is <span>some inline text</span> in a div.
   </div>
   ```

### 与其他元素的比较

- **与 `<div>` 的不同**：
  `<div>` 是块级元素，会导致换行并占据整行，而 `<span>` 是内联元素，不会换行，只占据其内容的宽度。

- **与 `<b>` 和 `<i>` 的不同**：
  `<b>` 和 `<i>` 元素用于粗体和斜体文本，它们有内置的样式效果，而 `<span>` 没有这些效果，只是一个通用的容器。

### 示例

1. **基本样式**：
   ```html
   <p>This is a <span style="color: blue; font-weight: bold;">blue and bold</span> text.</p>
   ```

2. **JavaScript 事件处理**：
   ```html
   <p>Click the <span id="clickMe">text here</span> to see an alert.</p>
   
   <script>
     document.getElementById('clickMe').addEventListener('click', () => {
       alert('Span clicked!');
     });
   </script>
   ```

3. **CSS 类应用**：
   ```html
   <p>Here is a <span class="highlight">highlighted</span> part of the text.</p>
   
   <style>
     .highlight {
       background-color: yellow;
       font-weight: bold;
     }
   </style>
   ```

总的来说，`<span>` 标签是一个非常灵活的工具，适用于需要在内联内容中应用样式或执行操作的场景。