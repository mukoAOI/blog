# Package org.apache.lucene.document



The logical representation of a [`Document`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/Document.html) for indexing and searching.

The document package provides the user level logical representation of content to be indexed and searched. The package also provides utilities for working with [`Document`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/Document.html)s and [`IndexableField`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexableField.html)s.

## Document and IndexableField

A [`Document`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/Document.html) is a collection of [`IndexableField`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexableField.html)s. A [`IndexableField`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexableField.html) is a logical representation of a user's content that needs to be indexed or stored. [`IndexableField`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexableField.html)s have a number of properties that tell Lucene how to treat the content (like indexed, tokenized, stored, etc.) See the [`Field`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/Field.html) implementation of [`IndexableField`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexableField.html) for specifics on these properties.

Note: it is common to refer to [`Document`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/Document.html)s having [`Field`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/Field.html)s, even though technically they have [`IndexableField`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexableField.html)s.

## Working with Documents

First and foremost, a [`Document`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/Document.html) is something created by the user application. It is your job to create Documents based on the content of the files you are working with in your application (Word, txt, PDF, Excel or any other format.) How this is done is completely up to you. That being said, there are many tools available in other projects that can make the process of taking a file and converting it into a Lucene [`Document`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/Document.html).

The [`DateTools`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/DateTools.html) is a utility class to make dates and times searchable. [`IntPoint`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/IntPoint.html), [`LongPoint`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/LongPoint.html), [`FloatPoint`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/FloatPoint.html) and [`DoublePoint`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/document/DoublePoint.html) enable indexing of numeric values (and also dates) for fast range queries using [`PointRangeQuery`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/search/PointRangeQuery.html)



包org.apache.lucene.document 用于索引和搜索的文档的逻辑表示。 document包提供了要索引和搜索的内容的用户级逻辑表示。该包还提供了用于处理文档和可索引字段的实用程序。

Document和IndexableField 文档是IndexableField的集合。IndexableField是用户内容的逻辑表示，需要进行索引或存储。IndexableFields具有一些属性，告诉Lucene如何处理内容（例如索引，标记化，存储等）。有关这些属性的具体信息，请参阅IndexableField的Field实现。

注意：通常提到文档具有字段，即使从技术上讲它们具有可索引字段。

处理文档 首先，文档是用户应用程序创建的东西。您的工作是根据应用程序中正在处理的文件的内容创建文档（Word，txt，PDF，Excel或任何其他格式）。如何完成这个任务完全取决于您。话虽如此，在其他项目中有许多可用工具可以将文件转换为Lucene文档的过程。

DateTools是一个实用类，用于使日期和时间可搜索。IntPoint、LongPoint、FloatPoint和DoublePoint使数值（以及日期）可以进行索引，以便使用PointRangeQuery进行快速范围查询。