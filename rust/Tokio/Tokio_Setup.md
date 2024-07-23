# Setup

本教程将逐步引导你构建一个Redis客户端和服务器。我们将从Rust的异步编程基础开始，并逐步深入。我们将实现Redis命令的一个子集，同时全面了解Tokio的使用。

# Mini-Redis

本教程中将构建的项目在GitHub上称为Mini-Redis。Mini-Redis的主要目标是学习Tokio，因此有着非常详细的注释。不过，由于这个目的，Mini-Redis缺少一些你在真实Redis库中可能需要的功能。你可以在crates.io上找到生产就绪的Redis库。

在教程中，我们将直接使用Mini-Redis。这使得我们可以在教程中先使用Mini-Redis的部分功能，然后在后续的教程中逐步实现它们。

# Getting Help

无论何时，如果你遇到困难，都可以在Discord或GitHub讨论区寻求帮助。不用担心提出“初学者”的问题。我们都曾经是从零开始，乐意提供帮助。

# Prerequisites

读者应该已经熟悉Rust。《Rust书》是一个很好的资源，可以帮助你入门。

虽然不是必需的，但一些使用Rust标准库或其他语言编写网络代码的经验会有所帮助。

不需要预先了解Redis。

### Rust

在开始之前，请确保您已安装并准备好Rust工具链。如果尚未安装，最简单的方法是使用rustup进行安装。

本教程要求至少使用Rust版本1.45.0，但建议使用最新的稳定版本。

要检查Rust是否已安装在您的计算机上，请运行以下

```cmd
rustc --version
```

你应该会看到类似 `rustc 1.46.0 (04488afe3 2020-08-24)` 的输出。

#### Mini-Redis服务器

 接下来，安装Mini-Redis服务器。我们将在构建客户端时用它来进行测试。

```cmd
cargo install mini-redis
```

确保服务器安装成功后，请启动它

```cmd
mini-redis-server
```

然后，在一个单独的终端窗口中，尝试使用 mini-redis-cli 获取键为 foo 的数据

```cmd
mini-redis-cli get foo
```

你应该会看到 (nil)。

# Ready to go

就这样，一切都准备就绪了。继续下一页，开始编写你的第一个异步 Rust 应用程序吧。

