**Package** [org.apache.lucene.store](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/package-summary.html)

## Class Directory

- [java.lang.Object](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Object.html?is-external=true)
- - org.apache.lucene.store.Directory

- - All Implemented Interfaces:

    `Closeable`, `AutoCloseable`

  - Direct Known Subclasses:

    `BaseDirectory`, `CompoundDirectory`, `FileSwitchDirectory`, `FilterDirectory`

  ------

  ```
  public abstract class Directory
  extends Object
  implements Closeable
  ```

  A `Directory` provides an abstraction layer for storing a list of files. A directory contains only files (no sub-folder hierarchy).

  Implementing classes must comply with the following:

  - A file in a directory can be created ([`createOutput(java.lang.String, org.apache.lucene.store.IOContext)`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/Directory.html#createOutput(java.lang.String,org.apache.lucene.store.IOContext))), appended to, then closed.
  - A file open for writing may not be available for read access until the corresponding [`IndexOutput`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/IndexOutput.html) is closed.
  - Once a file is created it must only be opened for input ([`openInput(java.lang.String, org.apache.lucene.store.IOContext)`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/Directory.html#openInput(java.lang.String,org.apache.lucene.store.IOContext))), or deleted ([`deleteFile(java.lang.String)`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/Directory.html#deleteFile(java.lang.String))). Calling [`createOutput(java.lang.String, org.apache.lucene.store.IOContext)`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/Directory.html#createOutput(java.lang.String,org.apache.lucene.store.IOContext)) on an existing file must throw [`FileAlreadyExistsException`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/FileAlreadyExistsException.html?is-external=true).

  **NOTE:** If your application requires external synchronization, you should **not** synchronize on the `Directory` implementation instance as this may cause deadlock; use your own (non-Lucene) objects instead.

  - **See Also:**

    [`FSDirectory`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/FSDirectory.html), [`ByteBuffersDirectory`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/ByteBuffersDirectory.html), [`FilterDirectory`](https://lucene.apache.org/core/9_10_0/core/org/apache/lucene/store/FilterDirectory.html)

包 org.apache.lucene.store 

类 Directory java.lang.Object org.apache.lucene.store.Directory 

已实现的所有接口： Closeable, AutoCloseable 

直接已知的子类： BaseDirectory, CompoundDirectory, FileSwitchDirectory, FilterDirectory

```
 public abstract class Directory 

extends Object

implements Closeable
```

 

Directory为存储文件列表提供了一个抽象层。目录仅包含文件（没有子文件夹层次结构）。 实现类必须符合以下规定：

1. 可以创建目录中的文件（createOutput(java.lang.String, org.apache.lucene.store.IOContext)），追加内容，然后关闭文件。
2. 打开用于写入的文件在关闭相应的 IndexOutput 之前可能不可用于读取访问。
3. 创建文件后，只能打开文件以进行读取访问（openInput(java.lang.String, org.apache.lucene.store.IOContext)），或删除文件（deleteFile(java.lang.String)）。对现有文件调用 createOutput(java.lang.String, org.apache.lucene.store.IOContext) 必须引发 FileAlreadyExistsException。 注意：如果您的应用程序需要外部同步，则不应该对 Directory 实现实例进行同步，因为这可能导致死锁；而应使用您自己的（非 Lucene）对象。

另请参阅： FSDirectory、ByteBuffersDirectory、FilterDirectory



Package org.apache.lucene.store
Class FSDirectory
java.lang.Object
org.apache.lucene.store.Directory
org.apache.lucene.store.BaseDirectory
org.apache.lucene.store.FSDirectory
All Implemented Interfaces:
Closeable, AutoCloseable
Direct Known Subclasses:
MMapDirectory, NIOFSDirectory
public abstract class FSDirectory
extends BaseDirectory
Base class for Directory implementations that store index files in the file system.  There are currently three core subclasses:
NIOFSDirectory uses java.nio's FileChannel's positional io when reading to avoid synchronization when reading from the same file. Unfortunately, due to a Windows-only Sun JRE bug this is a poor choice for Windows, but on all other platforms this is the preferred choice. Applications using Thread.interrupt() or Future.cancel(boolean) should use RAFDirectory instead. See NIOFSDirectory java doc for details.
MMapDirectory uses memory-mapped IO when reading. This is a good choice if you have plenty of virtual memory relative to your index size, eg if you are running on a 64 bit JRE, or you are running on a 32 bit JRE but your index sizes are small enough to fit into the virtual memory space. Java has currently the limitation of not being able to unmap files from user code. The files are unmapped, when GC releases the byte buffers. Due to this bug in Sun's JRE, MMapDirectory's IndexInput.close() is unable to close the underlying OS file handle. Only when GC finally collects the underlying objects, which could be quite some time later, will the file handle be closed. This will consume additional transient disk usage: on Windows, attempts to delete or overwrite the files will result in an exception; on other platforms, which typically have a "delete on last close" semantics, while such operations will succeed, the bytes are still consuming space on disk. For many applications this limitation is not a problem (e.g. if you have plenty of disk space, and you don't rely on overwriting files on Windows) but it's still an important limitation to be aware of. This class supplies a (possibly dangerous) workaround mentioned in the bug report, which may fail on non-Sun JVMs.
Unfortunately, because of system peculiarities, there is no single overall best implementation. Therefore, we've added the open(java.nio.file.Path) method, to allow Lucene to choose the best FSDirectory implementation given your environment, and the known limitations of each implementation. For users who have no reason to prefer a specific implementation, it's best to simply use open(java.nio.file.Path). For all others, you should instantiate the desired implementation directly.

NOTE: Accessing one of the above subclasses either directly or indirectly from a thread while it's interrupted can close the underlying channel immediately if at the same time the thread is blocked on IO. The channel will remain closed and subsequent access to the index will throw a ClosedChannelException. Applications using Thread.interrupt() or Future.cancel(boolean) should use the slower legacy RAFDirectory from the misc Lucene module instead.

The locking implementation is by default NativeFSLockFactory, but can be changed by passing in a custom LockFactory instance.



包org.apache.lucene.store 

类FSDirectory java.lang.Object org.apache.lucene.store.Directory org.apache.lucene.store.BaseDirectory org.apache.lucene.store.FSDirectory 

所有已实现的接口： Closeable, AutoCloseable 

直接已知的子类： MMapDirectory, NIOFSDirectory public abstract class FSDirectory extends BaseDirectory 

在文件系统中存储索引文件的Directory实现的基类。目前有三个核心子类： NIOFSDirectory在读取时使用java.nio的FileChannel的位置io，以避免从同一文件读取时的同步。不幸的是，由于Windows-only Sun JRE的bug，这对Windows来说是一个不好的选择，但在所有其他平台上，这是首选的选择。使用Thread.interrupt()或Future.cancel(boolean)的应用程序应该使用RAFDirectory。有关详细信息，请参阅NIOFSDirectory的java文档。 MMapDirectory在读取时使用内存映射IO。如果您的虚拟内存相对于索引大小足够多，例如如果您在64位JRE上运行，或者您在32位JRE上运行但您的索引大小足够小以适应虚拟内存空间，则这是一个很好的选择。Java目前有一个限制，即无法从用户代码中取消映射文件。当GC释放字节缓冲区时，文件会取消映射。由于Sun的JRE中存在此bug，MMapDirectory的IndexInput.close()无法关闭底层的OS文件句柄。只有当GC最终收集底层对象时，可能会相当长的时间，文件句柄才会关闭。这将消耗额外的瞬态磁盘使用量：在Windows上，尝试删除或覆盖文件将导致异常；在其他平台上，通常具有“在最后关闭时删除”的语义，虽然此类操作将成功，但字节仍在磁盘上占用空间。对于许多应用程序，这个限制不是问题（例如，如果您有大量的磁盘空间，并且您不依赖于在Windows上覆盖文件），但这仍然是一个重要的限制要注意。这个类提供了在bug报告中提到的（可能危险的）解决方法，该解决方法可能在非Sun JVM上失败。 不幸的是，由于系统的特殊性，没有单一的最佳实现。因此，我们添加了open(java.nio.file.Path)方法，以允许Lucene根据您的环境和每个实现的已知限制选择最佳的FSDirectory实现。对于没有理由偏好特定实现的用户，最好只是使用open(java.nio.file.Path)。对于所有其他用户，应该直接实例化所需的实现。

注意：如果在线程被中断的同时从线程直接或间接访问上述任何子类，并且在IO上被阻塞，那么通道将立即关闭。通道将保持关闭状态，并且后续对索引的访问将引发ClosedChannelException。使用Thread.interrupt()或Future.cancel(boolean)的应用程序应该使用misc Lucene模块中的较慢的传统RAFDirectory。

锁实现默认为NativeFSLockFactory，但可以通过传入自定义LockFactory实例来更改。



