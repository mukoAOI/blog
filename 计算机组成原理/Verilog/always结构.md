在 Verilog 中，`always` 块用于描述时序逻辑的行为，它定义了在特定条件下执行的硬件行为。`always` 块可以有不同的敏感性列表，决定了何时触发其中的代码执行。以下是 `always` 块的基本结构框架：

### 基本结构框架

```verilog
always @(posedge clk or negedge reset) begin
    // 这里是描述硬件行为的代码
end
```

### 解析

- **`always` 关键字**：用于声明一个时序逻辑块。
- **敏感性列表** (`@(posedge clk or negedge reset)`)：
  - `@(posedge clk)`：在时钟信号 `clk` 的上升沿触发。
  - `@(negedge reset)`：在复位信号 `reset` 的下降沿触发。
  - 敏感性列表可以包括多个信号或条件，用逗号分隔。

- **`begin` 和 `end`**：定义了 `always` 块中的代码段的开始和结束。

### 示例说明

```verilog
module Counter (
    input wire clk,
    input wire reset,
    output reg [7:0] count
);

    always @(posedge clk or negedge reset) begin
        if (!reset) begin
            count <= 8'b0;  // 在复位信号下降沿时将计数器清零
        end else begin
            count <= count + 1;  // 在时钟信号上升沿时递增计数器
        end
    end

endmodule
```

### 注意事项

- `always` 块内部的代码应当是组合逻辑或时序逻辑，具体取决于敏感性列表中的信号类型和操作。
- 对于时序逻辑，应当确保所有的状态转换和信号赋值都是在敏感性列表的信号变化时执行的，以保证正确的时序行为。
- 敏感性列表中的信号变化决定了 `always` 块中的代码执行时机，通常与模块的时钟和复位控制相关。

通过这种结构，Verilog 中的 `always` 块可以精确地描述出复杂的时序逻辑，是实现硬件描述中关键的语言结构之一。