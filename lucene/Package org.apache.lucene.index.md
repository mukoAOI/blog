# **Package** [org.apache.lucene.index](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/package-summary.html)

## Class IndexWriter

- [java.lang.Object](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Object.html?is-external=true)
- - org.apache.lucene.index.IndexWriter

- - All Implemented Interfaces:

    `Closeable`, `AutoCloseable`, `MergePolicy.MergeContext`, `TwoPhaseCommit`, `Accountable`

  ------

  ```
  public class IndexWriter
  extends Object
  implements Closeable, TwoPhaseCommit, Accountable, MergePolicy.MergeContext
  ```

  An `IndexWriter` creates and maintains an index.

  The [`IndexWriterConfig.OpenMode`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriterConfig.OpenMode.html) option on [`IndexWriterConfig.setOpenMode(OpenMode)`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriterConfig.html#setOpenMode(org.apache.lucene.index.IndexWriterConfig.OpenMode)) determines whether a new index is created, or whether an existing index is opened. Note that you can open an index with [`IndexWriterConfig.OpenMode.CREATE`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriterConfig.OpenMode.html#CREATE) even while readers are using the index. The old readers will continue to search the "point in time" snapshot they had opened, and won't see the newly created index until they re-open. If [`IndexWriterConfig.OpenMode.CREATE_OR_APPEND`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriterConfig.OpenMode.html#CREATE_OR_APPEND) is used IndexWriter will create a new index if there is not already an index at the provided path and otherwise open the existing index.

  In either case, documents are added with [`addDocument`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html#addDocument(java.lang.Iterable)) and removed with [`deleteDocuments(Term...)`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html#deleteDocuments(org.apache.lucene.index.Term...)) or [`deleteDocuments(Query...)`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html#deleteDocuments(org.apache.lucene.search.Query...)). A document can be updated with [`updateDocument`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html#updateDocument(org.apache.lucene.index.Term,java.lang.Iterable)) (which just deletes and then adds the entire document). When finished adding, deleting and updating documents, [`close`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html#close()) should be called. 

  Each method that changes the index returns a `long` sequence number, which expresses the effective order in which each change was applied. [`commit()`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html#commit()) also returns a sequence number, describing which changes are in the commit point and which are not. Sequence numbers are transient (not saved into the index in any way) and only valid within a single `IndexWriter` instance. 

  These changes are buffered in memory and periodically flushed to the [`Directory`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/Directory.html) (during the above method calls). A flush is triggered when there are enough added documents since the last flush. Flushing is triggered either by RAM usage of the documents (see [`IndexWriterConfig.setRAMBufferSizeMB(double)`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriterConfig.html#setRAMBufferSizeMB(double))) or the number of added documents (see [`IndexWriterConfig.setMaxBufferedDocs(int)`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriterConfig.html#setMaxBufferedDocs(int))). The default is to flush when RAM usage hits [`IndexWriterConfig.DEFAULT_RAM_BUFFER_SIZE_MB`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriterConfig.html#DEFAULT_RAM_BUFFER_SIZE_MB) MB. For best indexing speed you should flush by RAM usage with a large RAM buffer. In contrast to the other flush options [`IndexWriterConfig.setRAMBufferSizeMB(double)`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriterConfig.html#setRAMBufferSizeMB(double)) and [`IndexWriterConfig.setMaxBufferedDocs(int)`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriterConfig.html#setMaxBufferedDocs(int)), deleted terms won't trigger a segment flush. Note that flushing just moves the internal buffered state in IndexWriter into the index, but these changes are not visible to IndexReader until either [`commit()`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html#commit()) or [`close()`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html#close()) is called. A flush may also trigger one or more segment merges which by default run with a background thread so as not to block the addDocument calls (see [below](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html#mergePolicy) for changing the [`MergeScheduler`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/MergeScheduler.html)).

  Opening an `IndexWriter` creates a lock file for the directory in use. Trying to open another `IndexWriter` on the same directory will lead to a [`LockObtainFailedException`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/LockObtainFailedException.html). 

  Expert: `IndexWriter` allows an optional [`IndexDeletionPolicy`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexDeletionPolicy.html) implementation to be specified. You can use this to control when prior commits are deleted from the index. The default policy is [`KeepOnlyLastCommitDeletionPolicy`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/KeepOnlyLastCommitDeletionPolicy.html) which removes all prior commits as soon as a new commit is done. Creating your own policy can allow you to explicitly keep previous "point in time" commits alive in the index for some time, either because this is useful for your application, or to give readers enough time to refresh to the new commit without having the old commit deleted out from under them. The latter is necessary when multiple computers take turns opening their own `IndexWriter` and `IndexReader`s against a single shared index mounted via remote filesystems like NFS which do not support "delete on last close" semantics. A single computer accessing an index via NFS is fine with the default deletion policy since NFS clients emulate "delete on last close" locally. That said, accessing an index via NFS will likely result in poor performance compared to a local IO device. 

  Expert: `IndexWriter` allows you to separately change the [`MergePolicy`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/MergePolicy.html) and the [`MergeScheduler`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/MergeScheduler.html). The [`MergePolicy`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/MergePolicy.html) is invoked whenever there are changes to the segments in the index. Its role is to select which merges to do, if any, and return a [`MergePolicy.MergeSpecification`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/MergePolicy.MergeSpecification.html) describing the merges. The default is [`LogByteSizeMergePolicy`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/LogByteSizeMergePolicy.html). Then, the [`MergeScheduler`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/MergeScheduler.html) is invoked with the requested merges and it decides when and how to run the merges. The default is [`ConcurrentMergeScheduler`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/ConcurrentMergeScheduler.html). 

  **NOTE**: if you hit a VirtualMachineError, or disaster strikes during a checkpoint then IndexWriter will close itself. This is a defensive measure in case any internal state (buffered documents, deletions, reference counts) were corrupted. Any subsequent calls will throw an AlreadyClosedException. 

  **NOTE**: [`IndexWriter`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/index/IndexWriter.html) instances are completely thread safe, meaning multiple threads can call any of its methods, concurrently. If your application requires external synchronization, you should **not** synchronize on the `IndexWriter` instance as this may cause deadlock; use your own (non-Lucene) objects instead.

  **NOTE**: If you call `Thread.interrupt()` on a thread that's within IndexWriter, IndexWriter will try to catch this (eg, if it's in a wait() or Thread.sleep()), and will then throw the unchecked exception [`ThreadInterruptedException`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/util/ThreadInterruptedException.html) and **clear** the interrupt status on the thread.



包org.apache.lucene.index
类IndexWriter
java.lang.Object
org.apache.lucene.index.IndexWriter
所有已实现的接口：
Closeable, AutoCloseable, MergePolicy.MergeContext, TwoPhaseCommit, Accountable
public class IndexWriter
extends Object
implements Closeable, TwoPhaseCommit, Accountable, MergePolicy.MergeContext
IndexWriter创建并维护一个索引。
IndexWriterConfig.setOpenMode(OpenMode)上的IndexWriterConfig.OpenMode选项确定是创建新索引还是打开现有索引。请注意，即使读取器正在使用索引，您也可以使用IndexWriterConfig.OpenMode.CREATE打开索引。旧的读取器将继续搜索它们已经打开的“时间点”快照，并且直到它们重新打开才能看到新创建的索引。如果使用IndexWriterConfig.OpenMode.CREATE_OR_APPEND，则如果提供的路径处没有索引，则IndexWriter将创建新索引，否则将打开现有索引。

在任何情况下，都可以使用addDocument添加文档，并使用deleteDocuments(Term...)或deleteDocuments(Query...)删除文档。可以使用updateDocument更新文档（它只是删除然后添加整个文档）。在完成添加、删除和更新文档后，应调用close。

每个更改索引的方法都会返回一个长序列号，该序列号表示每个更改被应用的有效顺序。commit()也会返回一个序列号，描述哪些更改在提交点中，哪些不在。序列号是临时的（不以任何方式保存到索引中），并且仅在单个IndexWriter实例中有效。

这些更改在内存中缓冲，并定期刷新到Directory（在上述方法调用期间）。当自上次刷新以来添加了足够的文档时，将触发刷新。刷新是由文档的RAM使用量触发的（请参阅IndexWriterConfig.setRAMBufferSizeMB(double)）或已添加文档的数量触发的（请参阅IndexWriterConfig.setMaxBufferedDocs(int)）。默认情况下，当RAM使用量达到IndexWriterConfig.DEFAULT_RAM_BUFFER_SIZE_MB MB时会刷新。为了获得最佳的索引速度，您应该使用大的RAM缓冲区通过RAM使用量刷新。与其他刷新选项相反，删除的术语不会触发段刷新。请注意，刷新只是将IndexWriter中的内部缓冲状态移动到索引中，但在调用commit()或close()之前，这些更改对IndexReader不可见。刷新还可能触发一个或多个段合并，默认情况下，这些合并以后台线程方式运行，以免阻塞addDocument调用（有关更改MergeScheduler的详细信息，请参见下文）。

打开IndexWriter会为正在使用的目录创建一个锁文件。尝试在同一目录上打开另一个IndexWriter将导致LockObtainFailedException。

专家提示：IndexWriter允许指定一个可选的IndexDeletionPolicy实现。您可以使用这个来控制何时从索引中删除先前的提交。默认策略是KeepOnlyLastCommitDeletionPolicy，它在进行新提交后立即删除所有先前的提交。创建自己的策略可以让您明确地在索引中保留之前的“时间点”提交一段时间，这对于您的应用程序是有用的，或者为了给读取器足够的时间来刷新到新的提交，而不会删除旧的提交。在多台计算机轮流打开自己的IndexWriter和IndexReaders对单个共享索引进行操作时，后者是必需的，这些计算机通过不支持“在最后关闭时删除”的远程文件系统（如NFS）装载。通过NFS访问索引的单个计算机使用默认的删除策略是可以的，因为NFS客户端在本地模拟“在最后关闭时删除”。尽管如此，通过NFS访问索引可能会导致与本地IO设备相比性能较差。

专家提示：IndexWriter允许您单独更改MergePolicy和MergeScheduler。当索引中的段发生更改时，MergePolicy被调用。它的作用是选择要执行的合并（如果有的话），并返回描述合并的MergePolicy.MergeSpecification。默认值是LogByteSizeMergePolicy。然后，MergeScheduler被请求合并，并决定何时以及如何运行合并。默认值是ConcurrentMergeScheduler。

注意：如果在检查点期间发生VirtualMachineError或灾难，则IndexWriter将关闭自身。这是一种防御措施，以防任何内部状态（缓冲文档、删除、引用计数）被损坏。任何后续调用都将引发AlreadyClosedException。

注意：IndexWriter实例完全线程安全，这意味着多个线程可以同时调用它的任何方法。如果您的应用程序需要外部同步，您不应该对IndexWriter实例进行同步，因为这可能导致死锁；而应该使用您自己的（非Lucene）对象。 

注意：如果在IndexWriter中的线程上调用Thread.interrupt()，IndexWriter将尝试捕获这个（例如，如果它在wait()或Thread.sleep()中），然后会抛出未检查的异常ThreadInterruptedException，并在线程上清除中断状态。