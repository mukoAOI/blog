`<div>` 标签是 HTML 中一个非常常用的元素，主要用于将网页内容分组或组织成不同的区块。它是一个块级元素（block-level element），意味着它通常会开始于新的一行，并且占据整个可用的宽度。

**用途：**

1. **结构化网页：** `<div>` 标签常用于将网页内容分成逻辑上的区块，例如页面的头部、主体和底部。
   
2. **样式控制：** 使用 CSS 可以对 `<div>` 标签应用样式，例如设置背景色、边框、内外边距等。

3. **布局：** `<div>` 可以与 CSS Flexbox 或 Grid 布局一起使用，帮助设计响应式和复杂的页面布局。

**示例：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .header {
            background-color: lightblue;
            padding: 20px;
            text-align: center;
        }
        .main {
            padding: 20px;
            background-color: lightgreen;
        }
        .footer {
            background-color: lightcoral;
            padding: 10px;
            text-align: center;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>My Website</h1>
    </div>
    <div class="main">
        <p>Welcome to my website. Here is some content.</p>
    </div>
    <div class="footer">
        <p>Footer content here</p>
    </div>
</body>
</html>
```

在这个示例中，`<div>` 标签被用来创建头部、主体和底部区块，每个区块都有自己的样式。