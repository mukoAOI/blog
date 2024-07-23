# Hello Tokio

我们将从编写一个非常基本的Tokio应用程序开始。该程序将连接到Mini-Redis服务器，将键 hello 的值设置为 world。然后它会读取该键的值。这将使用Mini-Redis客户端库来完成。

# The code

### Generate a new crate

让我们首先生成一个新的Rust应用程序：

```bash
cargo new my-redis
cd my-redis
```

这将创建一个名为 `mini-redis-example` 的新的二进制 crate（即应用程序），你可以在这个 crate 中开始编写你的应用程序代码。

### Add dependencies

接下来，打开 `Cargo.toml` 文件，在 `[dependencies]` 下方添加以下内容：

```toml
tokio = { version = "1", features = ["full"] }
mini-redis = "0.4"
```

### Write the code

接下来，打开 `main.rs` 文件，并用以下内容替换文件中的内容

```rust
use mini_redis::{client, Result};

#[tokio::main]
async fn main() -> Result<()> {
    // Open a connection to the mini-redis address.
    let mut client = client::connect("127.0.0.1:6379").await?;

    // Set the key "hello" with value "world"
    client.set("hello", "world".into()).await?;

    // Get key "hello"
    let result = client.get("hello").await?;

    println!("got value from the server; result={:?}", result);

    Ok(())
}
```

确保 Mini-Redis 服务器正在运行。在一个单独的终端窗口中运行：

```bash
mini-redis-server
```

在前几张里有安装方式

outputs:

```
hello
world
```

一个异步函数的返回值是一个匿名类型，它实现了 Future trait。