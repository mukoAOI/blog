在 HTML 中，`<meta>` 标签用于提供关于 HTML 文档的元数据（即描述数据）。这些数据不会直接显示在页面上，但它们对于浏览器和搜索引擎非常重要。`<meta>` 标签通常放在 HTML 文档的 `<head>` 部分。

### 常见的 `<meta>` 标签属性和用法

1. **字符集声明**
   ```html
   <meta charset="UTF-8" />
   ```
   指定文档的字符编码，`UTF-8` 是一种广泛使用的编码，支持多种语言字符。

2. **视口设置**
   ```html
   <meta name="viewport" content="width=device-width, initial-scale=1.0" />
   ```
   这对响应式设计至关重要，控制页面在不同设备上的显示方式。

3. **描述信息**
   ```html
   <meta name="description" content="这是网页的描述信息。" />
   ```
   提供有关网页内容的简短描述，通常用于搜索引擎结果中的摘要。

4. **关键字**
   ```html
   <meta name="keywords" content="HTML, CSS, JavaScript, 编程" />
   ```
   包含与网页内容相关的关键字，虽然现代搜索引擎不再主要依赖此信息来排名，但仍有一些使用场景。

5. **作者**
   ```html
   <meta name="author" content="作者姓名" />
   ```
   指定网页的作者信息。

6. **网页刷新**
   ```html
   <meta http-equiv="refresh" content="30" />
   ```
   设置网页每隔指定时间（以秒为单位）自动刷新。

7. **页面重定向**
   ```html
   <meta http-equiv="refresh" content="5; url=https://example.com/" />
   ```
   在 5 秒后将页面重定向到指定的 URL。

8. **社交媒体预览**
   ```html
   <meta property="og:title" content="网页标题" />
   <meta property="og:description" content="网页描述" />
   <meta property="og:image" content="https://example.com/image.jpg" />
   ```
   Open Graph 协议用于定义网页在社交媒体分享时的显示方式。

### 总结

`<meta>` 标签提供了网页的额外信息，帮助浏览器和搜索引擎更好地处理和展示网页内容。它们在提高用户体验和优化搜索引擎结果方面发挥着重要作用。