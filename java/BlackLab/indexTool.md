### 使用 BlackLab 进行索引

**IndexTool** 是 BlackLab 的命令行工具，用于创建和管理文本语料库索引。

#### 基本操作：

1. **准备工作**：下载 或者自行打包BlackLab JAR 文件和库（放在名为 "lib" 的目录中）。
2. **启动 IndexTool**：运行以下命令查看帮助信息：
   ```bash
   java -cp "blacklab.jar:lib" nl.inl.blacklab.tools.IndexTool
   ```
   



#### ![image-20240824003927988](D:\blog\image-20240824003927988.png)创建索引：

```bash
java -cp "blacklab.jar:lib" nl.inl.blacklab.tools.IndexTool create INDEX_DIR INPUT_FILES FORMAT
```
- `INDEX_DIR`：索引存放目录。
- `INPUT_FILES`：要索引的文件或目录。
- `FORMAT`：文件格式（如 `tei`）。

#### 添加索引：
```bash
java -cp "blacklab.jar:lib" nl.inl.blacklab.tools.IndexTool add INDEX_DIR INPUT_FILES FORMAT
```

#### 删除文档：
```bash
java -cp "blacklab.jar:lib" nl.inl.blacklab.tools.IndexTool delete INDEX_DIR FILTER_QUERY
```
- `FILTER_QUERY`：Lucene 格式的查询，用于匹配要删除的文档。





注意再windows之中需要将：更换成；

#### 支持格式：
- 常见格式：`tei`, `csv`, `txt` 等。
- 可通过配置文件添加自定义格式。

#### 提高索引速度：
- 默认同时索引两个文档，可通过 `--threads n` 增加线程数。
