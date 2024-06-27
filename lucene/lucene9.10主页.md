Apache Lucene is a high-performance, full-featured text search engine library. Here's a simple example how to use Lucene for indexing and searching (using JUnit to check if the results are what we expect):

Apache Lucene是一个高性能、功能丰富的文本搜索引擎库。以下是如何使用Lucene进行索引和搜索的简单示例（使用JUnit来检查结果是否符合预期）：

```
    Analyzer analyzer = new StandardAnalyzer();

    Path indexPath = Files.createTempDirectory("tempIndex");
    Directory directory = FSDirectory.open(indexPath);
    IndexWriterConfig config = new IndexWriterConfig(analyzer);
    IndexWriter iwriter = new IndexWriter(directory, config);
    Document doc = new Document();
    String text = "This is the text to be indexed.";
    doc.add(new Field("fieldname", text, TextField.TYPE_STORED));
    iwriter.addDocument(doc);
    iwriter.close();
    
    // Now search the index:
    DirectoryReader ireader = DirectoryReader.open(directory);
    IndexSearcher isearcher = new IndexSearcher(ireader);
    // Parse a simple query that searches for "text":
    QueryParser parser = new QueryParser("fieldname", analyzer);
    Query query = parser.parse("text");
    ScoreDoc[] hits = isearcher.search(query, 10).scoreDocs;
    assertEquals(1, hits.length);
    // Iterate through the results:
    StoredFields storedFields = isearcher.storedFields();
    for (int i = 0; i < hits.length; i++) {
      Document hitDoc = storedFields.document(hits[i].doc);
      assertEquals("This is the text to be indexed.", hitDoc.get("fieldname"));
    }
    ireader.close();
    directory.close();
    IOUtils.rm(indexPath);
```



- **[`org.apache.lucene.analysis`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/analysis/package-summary.html)** defines an abstract [`Analyzer`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/analysis/Analyzer.html) API for converting text from a [`Reader`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/Reader.html?is-external=true) into a [`TokenStream`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/analysis/TokenStream.html), an enumeration of token [`Attribute`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/util/Attribute.html)s. A TokenStream can be composed by applying [`TokenFilter`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/analysis/TokenFilter.html)s to the output of a [`Tokenizer`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/analysis/Tokenizer.html). Tokenizers and TokenFilters are strung together and applied with an [`Analyzer`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/analysis/Analyzer.html). [analysis-common](https://lucene.apache.org/core/9_10_0/analysis/common/overview-summary.html) provides a number of Analyzer implementations, including [StopAnalyzer](https://lucene.apache.org/core/9_10_0/analysis/common/org/apache/lucene/analysis/core/StopAnalyzer.html) and the grammar-based [StandardAnalyzer](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/analysis/standard/StandardAnalyzer.html).
- **[`org.apache.lucene.codecs`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/codecs/package-summary.html)** provides an abstraction over the encoding and decoding of the inverted index structure, as well as different implementations that can be chosen depending upon application needs.
- **[`org.apache.lucene.document`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/package-summary.html)** provides a simple [`Document`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/Document.html) class. A Document is simply a set of named [`Field`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/Field.html)s, whose values may be strings or instances of [`Reader`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/Reader.html?is-external=true).
- **[`org.apache.lucene.index`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/package-summary.html)** provides two primary classes: [`IndexWriter`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html), which creates and adds documents to indices; and [`IndexReader`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexReader.html), which accesses the data in the index.
- **[`org.apache.lucene.search`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/search/package-summary.html)** provides data structures to represent queries (ie [`TermQuery`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/search/TermQuery.html) for individual words, [`PhraseQuery`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/search/PhraseQuery.html) for phrases, and [`BooleanQuery`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/search/BooleanQuery.html) for boolean combinations of queries) and the [`IndexSearcher`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/search/IndexSearcher.html) which turns queries into [`TopDocs`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/search/TopDocs.html). A number of [QueryParser](https://lucene.apache.org/core/9_10_0/queryparser/overview-summary.html)s are provided for producing query structures from strings or xml.
- **[`org.apache.lucene.store`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/package-summary.html)** defines an abstract class for storing persistent data, the [`Directory`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/Directory.html), which is a collection of named files written by an [`IndexOutput`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/IndexOutput.html) and read by an [`IndexInput`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/IndexInput.html). Multiple implementations are provided, but [`FSDirectory`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/FSDirectory.html) is generally recommended as it tries to use operating system disk buffer caches efficiently.
- **[`org.apache.lucene.util`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/util/package-summary.html)** contains a few handy data structures and util classes, ie [`FixedBitSet`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/util/FixedBitSet.html) and [`PriorityQueue`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/util/PriorityQueue.html).

To use Lucene, an application should:

1. Create [`Document`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/Document.html)s by adding [`Field`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/Field.html)s;
2. Create an [`IndexWriter`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html) and add documents to it with [`addDocument()`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html#addDocument(java.lang.Iterable));
3. Call [QueryParser.parse()](https://lucene.apache.org/core/9_10_0/queryparser/org/apache/lucene/queryparser/classic/QueryParserBase.html#parse(java.lang.String)) to build a query from a string; and
4. Create an [`IndexSearcher`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/search/IndexSearcher.html) and pass the query to its [`search()`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/search/IndexSearcher.html#search(org.apache.lucene.search.Query,int)) method.

Some simple examples of code which does this are:

-  [IndexFiles.java](https://lucene.apache.org/core/9_10_0/demo/src-html/org/apache/lucene/demo/IndexFiles.html) creates an index for all the files contained in

org.apache.lucene.analysis定义了一个抽象的Analyzer API，用于将文本从Reader转换为TokenStream，即标记属性的枚举。TokenStream可以通过将TokenFilters应用于Tokenizer的输出来组成。Tokenizer和TokenFilters被串联在一起，并通过Analyzer应用。analysis-common提供了许多Analyzer实现，包括StopAnalyzer和基于语法的StandardAnalyzer。

 org.apache.lucene.codecs提供了对倒排索引结构的编码和解码的抽象，以及可以根据应用需求选择的不同实现。 org.apache.lucene.document提供了一个简单的Document类。Document只是一组命名字段，其值可以是字符串或Reader的实例。 

org.apache.lucene.index提供了两个主要类：IndexWriter，用于创建并向索引添加文档；IndexReader，用于访问索引中的数据。 

org.apache.lucene.search提供了表示查询的数据结构（例如单词的TermQuery，短语的PhraseQuery和查询的布尔组合的BooleanQuery）和将查询转换为TopDocs的IndexSearcher。提供了一些QueryParsers，用于从字符串或XML生成查询结构。 

org.apache.lucene.store定义了用于存储持久数据的抽象类Directory，它是由IndexOutput编写并由IndexInput读取的一组命名文件的集合。提供了多种实现，但通常建议使用FSDirectory，因为它尝试有效地使用操作系统的磁盘缓存。

 org.apache.lucene.util包含一些方便的数据结构和工具类，例如FixedBitSet和PriorityQueue。

要使用Lucene，应用程序应该：

- 通过添加字段来创建文档；
- 创建一个IndexWriter，并使用addDocument()将文档添加到其中；
- 调用QueryParser.parse()从字符串构建查询；以及
- 创建一个IndexSearcher，并将查询传递给其search()方法。 一些简单的代码示例如下：
- IndexFiles.java：为目录中包含的所有文件创建索引。
- SearchFiles.java：提示用户输入查询并在索引中进行搜索。



Package Description org.apache.lucene.analysis 文本分析。

 org.apache.lucene.analysis.standard 快速、通用的基于语法的标记化器StandardTokenizer实现了Unicode文本分割算法的单词断句规则，如Unicode标准附录＃29中所规定的。

 org.apache.lucene.analysis.tokenattributes 用于文本分析的通用属性。

 org.apache.lucene.codecs 编解码器API：用于自定义索引的编码和结构。 

org.apache.lucene.codecs.compressing 压缩辅助类。 

org.apache.lucene.codecs.lucene90 Lucene 9.0文件格式。 

org.apache.lucene.codecs.lucene90.blocktree BlockTree词典。

 org.apache.lucene.codecs.lucene90.compressing Lucene 9.0压缩格式。

 org.apache.lucene.codecs.lucene94 Lucene 9.4文件格式。

 org.apache.lucene.codecs.lucene95 Lucene 9.5文件格式。

 org.apache.lucene.codecs.lucene99 Lucene 9.9文件格式。 

org.apache.lucene.codecs.perfield 可以根据字段委托到不同格式的倒排索引格式。

 org.apache.lucene.document 用于索引和搜索的文档的逻辑表示。 

org.apache.lucene.geo 用于Lucene Core的地理空间实用程序实现。

 org.apache.lucene.index 用于维护和访问索引的代码。

org.apache.lucene.internal.tests 内部桥接到包私有内部的测试框架，仅供lucene测试框架使用。 

org.apache.lucene.internal.vectorization 内部实现以支持SIMD矢量化。 

org.apache.lucene.search 用于搜索索引的代码。 

org.apache.lucene.search.comparators 比较器，用于比较命中以确定在收集TopFieldCollector的顶部结果时的排序顺序。 

org.apache.lucene.search.knn 与向量搜索相关的类：knn和向量字段。 

org.apache.lucene.search.similarities 此包包含可以在Lucene中使用的各种排名模型。 

org.apache.lucene.store 二进制I/O API，用于所有索引数据。 

org.apache.lucene.util 一些实用类。 

org.apache.lucene.util.automaton 用于正则表达式的有限状态自动机。 

org.apache.lucene.util.bkd 块KD树，实现了本文描述的通用空间数据结构。 

org.apache.lucene.util.compress 压缩实用程序。 

org.apache.lucene.util.fst 有限状态转换器。 

org.apache.lucene.util.graph 用于将标记流作为图形处理的实用程序类。 

org.apache.lucene.util.hnsw Navigable Small-World图，名义上是分层的，但当前只有一个图层。 

org.apache.lucene.util.hppc 保存hppc相关类的包。 

org.apache.lucene.util.mutable 可比较的对象包装器。 

org.apache.lucene.util.packed 打包的整数数组和流。 

org.apache.lucene.util.quantization 提供了将向量值缩放到更小的数据类型和可能更少维度的量化方法。