`<link>` 标签是HTML中的一个元素，用于在HTML文档中定义与外部资源的关系。它通常放置在文档的 `<head>` 部分，并且可以用于多种目的，包括引入样式表、设置图标、定义网站的配置信息等。

这里是 `<link>` 标签的一些常见用途：

1. **引入CSS样式表**:
   ```html
   <link rel="stylesheet" href="styles.css">
   ```
   这个用法将外部的CSS样式表文件（`styles.css`）链接到当前文档，从而应用样式。

2. **设置网页图标**:
   ```html
   <link rel="icon" type="image/png" href="favicon.png">
   ```
   这个用法将网页图标设置为 `favicon.png` 文件，浏览器会在标签页或书签中显示它。

3. **定义网站的描述和主题**:
   ```html
   <link rel="canonical" href="https://www.example.com/page.html">
   ```
   这个用法用于指定页面的规范URL，帮助搜索引擎理解页面的主要URL。

4. **设置网站的图标用于不同平台**:
   ```html
   <link rel="apple-touch-icon" sizes="180x180" href="apple-touch-icon.png">
   ```
   这个用法为苹果设备提供了专用的图标。

常见的 `rel` 属性值有：
- `stylesheet`：定义样式表。
- `icon`：定义网页图标。
- `canonical`：定义规范化的URL。
- `alternate`：定义备用资源。
- `preload`：定义预加载资源，以提高性能。

`<link>` 标签通常不直接显示在网页内容中，而是用来为文档提供附加的资源或元数据。