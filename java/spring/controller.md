# Controller





在Spring框架中，`Controller` 和 `RestController` 是用来处理 HTTP 请求的两种主要方式。

### 1. Controller

`Controller` 是一个注解，用于定义一个类作为Spring MVC应用程序中的控制器。它通常用于传统的Web应用程序，返回的是视图页面而不是数据。例如：

```java
@Controller
public class MyController {
    
    @RequestMapping("/hello")
    public String hello(Model model) {
        model.addAttribute("message", "Hello, World!");
        return "helloPage"; // 返回视图名
    }
}
```

在上面的例子中，`MyController` 类处理了一个 `/hello` 的请求，并返回一个名为 `helloPage` 的视图页面。

### 2. RestController

`RestController` 是 `Controller` 的一个特殊版本，它使得编写 RESTful Web 服务变得更加简单。它将类标记为处理 REST 请求并返回数据，而不是视图。返回的数据通常是 JSON 格式。例如：

```java
@RestController
public class MyRestController {
    
    @GetMapping("/api/hello")
    public String hello() {
        return "Hello, World!";
    }
}
```

在这个例子中，`MyRestController` 类处理了一个 `GET` 请求 `/api/hello`，并返回一个字符串 "Hello, World!"，这个字符串会被自动转换为 JSON 格式作为响应返回给客户端。

### 区别总结

- **返回类型**：
  - `Controller` 返回视图页面，通常使用 `String` 来表示视图名。
  - `RestController` 返回数据（如 JSON、XML 等），Spring 自动将返回的对象序列化为响应体。

- **默认行为**：
  - `Controller` 方法默认会使用 `@ResponseBody` 将返回结果写入响应体，但通常返回的是视图名称。
  - `RestController` 方法默认行为就是将方法的返回值直接写入响应体，通常是将对象转为 JSON。

- **常用场景**：
  - 如果开发的是传统的 Web 应用，使用 `Controller` 更合适。
  - 如果开发的是 RESTful 服务，使用 `RestController` 更方便，因为它更专注于处理数据而不是视图。

综上所述，选择 `Controller` 还是 `RestController` 取决于你的应用是传统的 Web 应用还是 RESTful 服务，以及你需要返回的是视图还是数据。





## 什么是RESTful web

RESTful Web 是一种设计风格或架构模式，旨在提升 Web 服务的互操作性和可伸缩性。它基于 REST（Representational State Transfer）原则，这些原则是由 Roy Fielding 在他的博士论文中提出的，并成为了现代互联网架构的重要基础之一。

### 特点和原则

RESTful Web 服务具有以下特点和原则：

1. **基于资源**：在 REST 中，资源（Resource）是指网络上的一个实体或服务的抽象，可以是文本、图片、视频等。每个资源都有一个唯一的标识符（URI），客户端通过 URI 访问资源。

2. **统一接口**：RESTful 服务的核心是其统一的接口，包括使用标准的 HTTP 方法（GET、POST、PUT、DELETE 等）对资源进行操作，以及通过 URI 表示资源。

3. **无状态性**：服务器不保存客户端请求的状态，每个请求都应包含足够的信息以便服务器理解请求。会话状态应该完全保存在客户端。

4. **资源的表现层**：客户端可以请求不同的表现层（Representation），例如 JSON、XML 等，通过 HTTP 响应来表示资源的当前状态。

5. **超媒体驱动（HATEOAS）**：即 Hypermedia As The Engine Of Application State，通过在响应中提供相关资源的链接，使客户端能够动态地发现和使用服务。这种机制提升了服务的自描述性和可发现性。

### 设计原则

RESTful Web 服务的设计原则包括：

- 使用合适的 HTTP 方法来执行操作：GET 用于获取资源，POST 用于创建资源，PUT 或 PATCH 用于更新资源，DELETE 用于删除资源等。
- 使用 URI 来唯一标识资源，每个资源都有一个独特的地址。
- 通过状态码来传达操作结果和错误信息。
- 提供资源的多种表现形式（多种格式的数据如 JSON、XML）。

### 优点

采用 RESTful 设计的 Web 服务具有以下优点：

- **可伸缩性和性能**：RESTful 架构支持水平扩展，每个请求都是独立的，不依赖于服务器的状态信息，易于实现负载均衡和缓存机制。
  
- **简化开发和集成**：RESTful 接口基于 HTTP 标准，使用简单且广泛支持的技术，如 JSON 和 XML，使得不同平台和语言的系统能够轻松集成。

- **增强的可靠性和可维护性**：通过明确的接口规范和资源标识，降低了系统的耦合度，提升了系统的可靠性和可维护性。

总结来说，RESTful Web 服务通过其简单的设计原则和标准化的接口，提供了一种高效、灵活且易于集成的方式来构建和管理分布式系统和服务。







除了 `@RestController`，在Spring框架中还有一些其他常用的用来处理HTTP请求的注解标签，每个标签都有其特定的用途和场景。

### 1. `@Controller`

`@Controller` 注解用于定义一个类作为Spring MVC应用程序中的控制器。它主要用于传统的Web应用程序，通常返回视图页面而不是数据。例如：

```java
@Controller
public class MyController {
    
    @RequestMapping("/hello")
    public String hello(Model model) {
        model.addAttribute("message", "Hello, World!");
        return "helloPage"; // 返回视图名
    }
}
```

### 2. `@RequestMapping`

`@RequestMapping` 用于将 HTTP 请求映射到具体的控制器方法或者类。它可以用在类级别和方法级别。在类级别上可以指定根路径，在方法级别上可以指定具体的路径和请求方法。例如：

```java
@Controller
@RequestMapping("/example")
public class ExampleController {
    
    @RequestMapping(value = "/hello", method = RequestMethod.GET)
    public String hello(Model model) {
        model.addAttribute("message", "Hello, World!");
        return "helloPage"; // 返回视图名
    }
}
```

### 3. `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`

这些注解分别用于处理 GET、POST、PUT 和 DELETE 请求。它们是 `@RequestMapping(method=...)` 的快捷方式。例如：

```java
@RestController
@RequestMapping("/api")
public class MyRestController {
    
    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }
    
    @PostMapping("/create")
    public ResponseEntity<String> create() {
        // 处理 POST 请求
    }
    
    @PutMapping("/update/{id}")
    public ResponseEntity<String> update(@PathVariable Long id) {
        // 处理 PUT 请求
    }
    
    @DeleteMapping("/delete/{id}")
    public ResponseEntity<String> delete(@PathVariable Long id) {
        // 处理 DELETE 请求
    }
}
```

### 4. `@PathVariable`

`@PathVariable` 用于从 URI 中提取模板变量的值，并将其绑定到方法参数上。例如：

```java
@GetMapping("/books/{id}")
public ResponseEntity<Book> getBookById(@PathVariable Long id) {
    // 根据 id 获取书籍信息
}
```

### 5. `@RequestBody` 和 `@ResponseBody`

- `@RequestBody` 用于将 HTTP 请求的内容（如 JSON）绑定到方法的参数上，常用于处理 POST 请求中的请求体数据。
  
- `@ResponseBody` 用于将方法的返回值直接写入 HTTP 响应体中，常用于 RESTful 服务返回 JSON 或 XML 数据。

```java
@PostMapping("/create")
public ResponseEntity<Book> createBook(@RequestBody Book book) {
    // 处理传入的 Book 对象数据
}
```

### 总结

除了 `@RestController`，Spring MVC 还提供了许多其他的注解，用于帮助开发者更精确地控制和处理 HTTP 请求和响应。这些注解使得开发 RESTful Web 服务和传统的 Web 应用程序变得更加简单和灵活。