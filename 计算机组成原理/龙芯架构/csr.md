

![image-20240718143840337](D:\blog\计算机组成原理\龙芯架构\assets\image-20240718143840337.png)







这个Verilog模块名为`CSR`，看起来是一个复杂的控制与状态寄存器单元，设计用于处理处理器或类似数字系统的各个方面。让我们根据提供的端口和注释来逐步分析其组成部分：

### 输入
- **CLK**: 同步操作的时钟信号。
- **Reset**: 复位信号，用于复位内部状态。
- **interrupt**: 9位输入，用于处理中断。
- **csr_raddr**: 14位地址，用于读取CSR值。
- **csr_we**: 写使能信号，用于写入CSR。
- **csr_wvalue**: 要写入CSR的32位数据。
- **csr_waddr**: 14位地址，用于写入CSR值。
- **ecode_in**: 6位输入，用于异常代码。
- **esubcode_in**: 9位输入，用于异常子代码。
- **excp_PC**: 异常发生时的32位程序计数器。
- **va_error_in**: 输入信号，指示地址错误异常。
- **bad_va_in**: 32位数据，指示导致错误的坏虚拟地址。
- **excp_tlbrefill, llbit_in, llbit_set_in, excp_flush, ertn_flush, tlbsrch_en, tlbsrch_found, tlbsrch_index, excp_tlb, excp_tlb_vppn**: 与异常、TLB（地址转换缓冲）操作和其他异常处理机制相关的各种控制信号。

### 输出
- **eentry_out**: 异常处理程序入口地址的32位输出。
- **era_out**: 与异常处理相关的32位输出（从提供的片段来看具体用途不明确）。
- **has_int**: 表示是否有待处理中断的输出。
- **csr_rvalue**: 从CSR读取的32位值的输出。
- **plv_out**: 表示特权级别的2位输出。
- **tid_out**: 表示计时器标识符的32位输出。
- **stable_counter_out**: 稳定计数器值的64位输出。
- **asid_out, rand_index, tlbehi_out, tlbelo0_out, tlbelo1_out, tlbidx_out, pg_out, da_out, dmw0_out, dmw1_out, datf_out, datm_out, ecode_out, vppn_out**: 与地址转换和内存管理（TLB条目、虚拟地址到物理地址转换的详细信息）相关的输出。

### 其他
- **disable_cache_out**: 与缓存控制相关的输出。
- **tlbrentry_out**: 与TLB条目相关的32位输出。

### 注释
模块中的注释提供了对一些信号和输出的简要描述，暗示了它们在系统内部的预期用途或功能。然而，每个输入和输出的确切功能和目的，将严重依赖于此模块所属处理器或系统的具体架构和设计目标。

### 总结
总之，这个Verilog模块`CSR`包含了一系列复杂的功能，包括处理中断、管理控制和状态寄存器（CSR）、支持异常处理、支持用于内存管理的TLB操作，并可能与缓存控制机制进行接口。每个输入和输出在整个系统中协调这些操作的特定角色。





这段Verilog代码定义了一些寄存器和参数，用于处理控制与状态寄存器（CSR）以及一些局部参数的逻辑。

### 寄存器定义
```verilog
reg[31:0] CSR_CRMD;         // 控制模式寄存器
reg[31:0] CSR_PRMD;         // 特权模式寄存器
reg[31:0] CSR_ECFG;         // 异常配置寄存器
reg[31:0] CSR_EXFG;         // 异常标志寄存器
reg[31:0] CSR_ESTAT;        // 异常状态寄存器
reg[31:0] CSR_ERA;          // 异常返回地址寄存器
reg[31:0] CSR_BADV;         // 错误地址寄存器
reg[31:0] CSR_EENTRY;       // 异常处理入口地址寄存器
reg[31:0] csr_tlbidx;       // TLB索引寄存器
reg[31:0] csr_tlbehi;       // TLB高位寄存器
reg[31:0] csr_tlbelo0;      // TLB低位0寄存器
reg[31:0] csr_tlbelo1;      // TLB低位1寄存器
reg[31:0] csr_asid;         // 地址空间标识寄存器
reg[31:0] csr_tlbrentry;    // TLB条目寄存器
reg[31:0] csr_dmw0;         // 数据修改寄存器0
reg[31:0] csr_dmw1;         // 数据修改寄存器1
reg[31:0] csr_disable_cache;// 禁用缓存寄存器
reg[31:0] CSR_CPUID;        // CPU标识寄存器
reg[31:0] CSR_SAVE0;        // 保存寄存器0
reg[31:0] CSR_SAVE1;        // 保存寄存器1
reg[31:0] CSR_SAVE2;        // 保存寄存器2
reg[31:0] CSR_SAVE3;        // 保存寄存器3
reg[31:0] CSR_LLBCTL;       // LLB控制寄存器
reg[31:0] CSR_TID;          // 计时器ID寄存器
reg[31:0] CSR_TCFG;         // 计时器配置寄存器
reg[31:0] CSR_TVAL;         // 计时器值寄存器
reg[31:0] CSR_TICLR;        // 计时器清除寄存器
```

### Wire定义
```verilog
wire eret_tlbrefill_excp;
assign eret_tlbrefill_excp = CSR_ESTAT[`ECODE] == 6'h3f;
```

### 本地参数定义
```verilog
localparam CRMD = 14'h0;
localparam PRMD = 14'h1;
localparam ECFG = 14'h4;
localparam ESTAT = 14'h5;
localparam ERA = 14'h6;
localparam BADV = 14'h7;
localparam EENTRY = 14'hc;
localparam TLBIDX= 14'h10;
localparam TLBEHI= 14'h11;
localparam TLBELO0=14'h12;
localparam TLBELO1=14'h13;
localparam ASID = 14'h18;
localparam PGDL = 14'h19;
localparam PGDH  = 14'h1a;
localparam PGD = 14'h1b;
localparam CPUID = 14'h20;
localparam SAVE0 = 14'h30;
localparam SAVE1 = 14'h31;
localparam SAVE2 = 14'h32;
localparam SAVE3 = 14'h33;
localparam TID = 14'h40;
localparam TCFG = 14'h41;
localparam TVAL = 14'h42;
localparam TICLR = 14'h44;
localparam LLBCTL= 14'h60;
localparam TLBRENTRY = 14'h88;
localparam DMW0 = 14'h180;
localparam DMW1 = 14'h181;
localparam BRK = 14'h100;
localparam DISABLE_CACHE = 14'h101;
```

### 注释和总结
这些寄存器和参数的定义用于在Verilog中实现处理器或者类似的数字系统的功能。它们涵盖了控制模式、特权模式、异常处理、TLB管理、计时器和缓存等方面的功能和状态。本地参数提供了每个寄存器的地址标识符，用于在设计中引用这些寄存器。





这段Verilog代码定义了一系列的wire和assign语句，用于管理和操作多个控制与状态寄存器（CSR）。以下是代码中涉及的一些关键部分的汉语解释和注释：

### 写使能逻辑
```verilog
wire crmd_wen = csr_we & (csr_waddr == CRMD);               // 写CRMD寄存器使能
wire prmd_wen = csr_we & (csr_waddr == PRMD);               // 写PRMD寄存器使能
wire ecfg_wen = csr_we & (csr_waddr == ECFG);               // 写ECFG寄存器使能
wire estat_wen  = csr_we & (csr_waddr == ESTAT);            // 写ESTAT寄存器使能
wire era_wen    = csr_we & (csr_waddr == ERA);              // 写ERA寄存器使能
wire badv_wen   = csr_we & (csr_waddr == BADV);             // 写BADV寄存器使能
wire eentry_wen = csr_we & (csr_waddr == EENTRY);           // 写EENTRY寄存器使能
wire tlbidx_wen = csr_we & (csr_waddr == TLBIDX);           // 写TLBIDX寄存器使能
wire tlbehi_wen = csr_we & (csr_waddr == TLBEHI);           // 写TLBEHI寄存器使能
wire tlbelo0_wen= csr_we & (csr_waddr == TLBELO0);          // 写TLBELO0寄存器使能
wire tlbelo1_wen= csr_we & (csr_waddr == TLBELO1);          // 写TLBELO1寄存器使能
wire asid_wen   = csr_we & (csr_waddr == ASID);             // 写ASID寄存器使能
wire pgdl_wen   = csr_we & (csr_waddr == PGDL);             // 写PGDL寄存器使能
wire pgdh_wen   = csr_we & (csr_waddr == PGDH);             // 写PGDH寄存器使能
wire pgd_wen    = csr_we & (csr_waddr == PGD);              // 写PGD寄存器使能
wire cpuid_wen  = csr_we & (csr_waddr == CPUID);            // 写CPUID寄存器使能
wire save0_wen  = csr_we & (csr_waddr == SAVE0);            // 写SAVE0寄存器使能
wire save1_wen  = csr_we & (csr_waddr == SAVE1);            // 写SAVE1寄存器使能
wire save2_wen  = csr_we & (csr_waddr == SAVE2);            // 写SAVE2寄存器使能
wire save3_wen  = csr_we & (csr_waddr == SAVE3);            // 写SAVE3寄存器使能
wire tid_wen    = csr_we & (csr_waddr == TID);              // 写TID寄存器使能
wire tcfg_wen   = csr_we & (csr_waddr == TCFG);             // 写TCFG寄存器使能
wire tval_wen   = csr_we & (csr_waddr == TVAL);             // 写TVAL寄存器使能
wire ticlr_wen  = csr_we & (csr_waddr == TICLR);            // 写TICLR寄存器使能
wire llbctl_wen = csr_we & (csr_waddr == LLBCTL);           // 写LLBCTL寄存器使能
wire tlbrentry_wen = csr_we & (csr_waddr == TLBRENTRY);     // 写TLBRENTRY寄存器使能
wire DMW0_wen   = csr_we & (csr_waddr == DMW0);             // 写DMW0寄存器使能
wire DMW1_wen   = csr_we & (csr_waddr == DMW1);             // 写DMW1寄存器使能
wire BRK_wen    = csr_we & (csr_waddr == BRK);              // 写BRK寄存器使能
wire disable_cache_wen = csr_we & (csr_waddr == DISABLE_CACHE); // 写DISABLE_CACHE寄存器使能
```

### 寄存器地址本地参数
```verilog
localparam CRMD = 14'h0;          // CRMD寄存器地址
localparam PRMD = 14'h1;          // PRMD寄存器地址
localparam ECFG = 14'h4;          // ECFG寄存器地址
localparam ESTAT = 14'h5;         // ESTAT寄存器地址
localparam ERA = 14'h6;           // ERA寄存器地址
localparam BADV = 14'h7;          // BADV寄存器地址
localparam EENTRY = 14'hc;        // EENTRY寄存器地址
localparam TLBIDX= 14'h10;        // TLBIDX寄存器地址
localparam TLBEHI= 14'h11;        // TLBEHI寄存器地址
localparam TLBELO0=14'h12;        // TLBELO0寄存器地址
localparam TLBELO1=14'h13;        // TLBELO1寄存器地址
localparam ASID = 14'h18;         // ASID寄存器地址
localparam PGDL = 14'h19;         // PGDL寄存器地址
localparam PGDH  = 14'h1a;        // PGDH寄存器地址
localparam PGD = 14'h1b;          // PGD寄存器地址
localparam CPUID = 14'h20;        // CPUID寄存器地址
localparam SAVE0 = 14'h30;        // SAVE0寄存器地址
localparam SAVE1 = 14'h31;        // SAVE1寄存器地址
localparam SAVE2 = 14'h32;        // SAVE2寄存器地址
localparam SAVE3 = 14'h33;        // SAVE3寄存器地址
localparam TID = 14'h40;          // TID寄存器地址
localparam TCFG = 14'h41;         // TCFG寄存器地址
localparam TVAL = 14'h42;         // TVAL寄存器地址
localparam TICLR = 14'h44;        // TICLR寄存器地址
localparam LLBCTL= 14'h60;        // LLBCTL寄存器地址
localparam TLBRENTRY = 14'h88;    // TLBRENTRY寄存器地址
localparam DMW0 = 14'h180;        // DMW0寄存器地址
localparam DMW1 = 14'h181;        // DMW1寄存器地址
localparam BRK = 14'h100;         // BRK寄存器地址
localparam DISABLE_CACHE = 14'h101; // DISABLE_CACHE寄存器地址
```

### assign语句
```verilog
assign asid_out = csr_asid[`TLB_ASID];                      // 输出TLB的ASID字段
assign tlbehi_out   = csr_tlbehi;                           // 输出TLB的高位字段
assign tlbelo0_out  = csr_tlbelo0;                          // 输出TLB的低位0字段
assign tlbelo1_out  = csr_tlbelo1;                          // 输出TLB的低位1字段
assign tlbidx_out   = csr_tlbidx;                           // 输出TLB的索引字段
assign tlbrentry_out = csr_tlbrentry;                       // 输出TLB的条目字段
assign rand_index   = Stable_Counter[4:0];                  // 输出稳定计数器的低5位
assign disable_cache_out = csr_disable_cache[0];            // 输出DISABLE_CACHE寄存器的最低位
assign datf_out     = CSR_CRMD[`DATF];                      // 输出CRMD寄存器的DATF字段
assign datm_out     = CSR_CRMD[`DATM];                      // 输出CRMD寄存器的DATM字段
assign ecode_out    = CSR_ESTAT[`ECODE];                    // 输出ESTAT寄存器的ECODE字段
assign vppn_out     = tlbehi_wen ? csr_wvalue[`VPPN] : csr_tlbehi[`VPPN]; // 根据tlbehi_wen确定输出VPPN字段
assign dmw0_out     = DMW0_wen ? csr_wvalue : csr_dmw0;     // 根据DMW0_wen确定输出DMW0字段
assign dmw1_out     = DMW1
```











当然可以，以下是你提供的Verilog代码中各个部分的功能描述：

1. **CSR_CRMD (CRMD控制和状态寄存器)**：管理特权级（`PLV`）、中断使能（`IE`）、数据访问（`DA`）、页面全局（`PG`）等各种控制和状态位。它在复位时初始化，并根据异常处理（`excp_flush`）、异常返回（`ertn_flush`）或写操作（`crmd_wen`）更新其值。

2. **CSR_PRMD (PRMD控制和状态寄存器)**：管理特权级（`PPLV`）和中断使能（`PIE`）状态。它在复位时初始化，并根据异常处理（`excp_flush`）或写操作（`prmd_wen`）更新其值。

3. **CSR_ECFG (ECFG控制和状态寄存器)**：处理与中断相关的配置位，特别是中断使能（`LIE`）。它在复位时初始化，并在写操作（`ecfg_wen`）时更新。

4. **CSR_ESTAT (ESTAT控制和状态寄存器)**：管理异常相关的位（如`ECODE`、`ESUBCODE`）、定时器使能（`EN`）和状态（定时器中断）。它在复位时初始化，在定时器操作和异常处理时更新，还可以在写操作（`estat_wen`）时修改某些位。

5. **CSR_ERA (ERA控制和状态寄存器)**：在异常处理时记录程序计数器（`PC`），并在写操作（`era_wen`）时更新。

6. **CSR_BADV (BADV控制和状态寄存器)**：处理与错误虚拟地址相关的异常，如在写操作（`badv_wen`）时更新。

7. **CSR_EENTRY (EENTRY控制和状态寄存器)**：管理异常和中断的入口点。它在复位时初始化，并在写操作（`eentry_wen`）时更新。

8. **CSR_SAVE0-3 (SAVE0-3控制和状态寄存器)**：用于保存异常处理过程中的上下文。它们在复位时初始化，并分别在对应的写操作（`save0_wen`、`save1_wen`、`save2_wen`、`save3_wen`）时更新。

9. **CSR_LLBCTL (LLBCTL控制和状态寄存器)**：管理Load Linked/Store Conditional（LL/SC）操作相关的控制。它在复位时初始化，并在写操作（`llbctl_wen`）时更新。

每个寄存器块都根据时钟信号（`CLK`）同步更新，并在复位信号（`Reset`）时进行初始化，确保在处理器设计中正确地管理和更新各种控制和状态。







这段Verilog代码是一个处理器中的控制和状态寄存器模块的实现。让我们逐段来讲解每个部分的功能和作用。

### 1. CSR_CRMD (基础控制状态寄存器)

```verilog
always @ (posedge CLK) begin
    if (Reset) begin
        // 初始化操作
        CSR_CRMD[`PLV] <=  2'b0;
        CSR_CRMD[`IE] <=  1'b0;
        CSR_CRMD[`DA] <=  1'b1;
        CSR_CRMD[`PG] <=  1'b0;
        CSR_CRMD[`DATF] <=  2'b0;
        CSR_CRMD[`DATM] <=  2'b0;
        CSR_CRMD[31:9] <=  23'b0;
    end
    else if (excp_flush) begin
        // 触发例外时更新状态
        CSR_CRMD[`PLV] <=  2'b0;
        CSR_CRMD[`IE] <=  1'b0;
        if (excp_tlbrefill) begin
            CSR_CRMD[`DA] <=  1'b1;
            CSR_CRMD[`PG] <=  1'b0;
        end
    end
    else if (ertn_flush) begin
        // 执行ertn命令时更新状态
        CSR_CRMD[`PLV] <=  CSR_PRMD[`PPLV];
        CSR_CRMD[`IE] <=  CSR_PRMD[`PIE];
        if (eret_tlbrefill_excp) begin
            CSR_CRMD[`DA] <=  1'b0;
            CSR_CRMD[`PG] <=  1'b1;
        end
    end 
    else if (crmd_wen) begin
        // 写操作时更新状态
        CSR_CRMD[`PLV] <=  csr_wvalue[`PLV];
        CSR_CRMD[`IE] <=  csr_wvalue[`IE];
        CSR_CRMD[`DA] <=  csr_wvalue[`DA];
        CSR_CRMD[`PG] <=  csr_wvalue[`PG];
        CSR_CRMD[`DATF] <=  csr_wvalue[`DATF];
        CSR_CRMD[`DATM] <=  csr_wvalue[`DATM];
    end
end
```

- **功能说明**：
  - **Reset**：在复位时，初始化`CSR_CRMD`寄存器的各个字段。
  - **excp_flush**：当发生例外时，根据不同的例外类型更新寄存器状态。
  - **ertn_flush**：执行ERTN指令时，将部分状态从`CSR_PRMD`寄存器中拷贝到`CSR_CRMD`寄存器。
  - **crmd_wen**：当写使能信号有效时，将输入的写值更新到`CSR_CRMD`寄存器的相应字段。

### 2. CSR_PRMD (异常处理状态寄存器)

```verilog
always @(posedge CLK) begin
    if (Reset) begin
        CSR_PRMD[31:3] <=  29'b0;
        CSR_PRMD[2:0] <=  3'b0; // 保留埿
    end
    else if (excp_flush) begin
        // 触发例外时更新状态
        CSR_PRMD[`PPLV] <=  CSR_CRMD[`PLV];
        CSR_PRMD[`PIE] <=  CSR_CRMD[`IE];
    end
    else if (prmd_wen) begin
        // 写操作时更新状态
        CSR_PRMD[`PPLV] <=  csr_wvalue[`PPLV];
        CSR_PRMD[`PIE] <=  csr_wvalue[`PIE];
    end
end
```

- **功能说明**：
  - **Reset**：在复位时，初始化`CSR_PRMD`寄存器的各个字段。
  - **excp_flush**：当发生例外时，将部分状态从`CSR_CRMD`寄存器拷贝到`CSR_PRMD`寄存器。
  - **prmd_wen**：当写使能信号有效时，将输入的写值更新到`CSR_PRMD`寄存器的相应字段。

### 3. CSR_ECFG (异常配置寄存器)

```verilog
always @(posedge CLK) begin
    if (Reset) begin
        CSR_ECFG <=  32'b0;
    end
    else if (ecfg_wen) begin
        // 写操作时更新状态
        CSR_ECFG[`LIE] <=  csr_wvalue[`LIE];
    end
end
```

- **功能说明**：
  - **Reset**：在复位时，初始化`CSR_ECFG`寄存器。
  - **ecfg_wen**：当写使能信号有效时，将输入的写值更新到`CSR_ECFG`寄存器的相应字段。

### 4. CSR_ESTAT (异常状态寄存器)

```verilog
always @(posedge CLK) begin
    if (Reset) begin
        CSR_ESTAT <=  32'b0;
        timer_en <=  1'b0;
    end
    else begin
        if (ticlr_wen && csr_wvalue[`CLR]) begin
            CSR_ESTAT[11] <=  1'b0;
        end
        else if (tcfg_wen) begin
            timer_en <=  csr_wvalue[`EN]; // 计时器使能
        end
        else if (timer_en && (CSR_TVAL ==  32'b0)) begin
            CSR_ESTAT[11] <=  1'b1;
            timer_en <=  CSR_TCFG[`PERIODIC];
            CSR_TCFG <=  CSR_TCFG[`PERIODIC];
        end
        // CSR_ESTAT[10:2] <=  interrupt;
        if (excp_flush) begin
            // 触发例外时更新状态
            CSR_ESTAT[`ECODE] <=  ecode_in;
            CSR_ESTAT[`ESUBCODE] <=  esubcode_in;
        end
        else if (estat_wen) begin
            // 写操作时更新状态
            CSR_ESTAT[1:0] <=  csr_wvalue[1:0];
        end
    end
end
```

- **功能说明**：
  - **Reset**：在复位时，初始化`CSR_ESTAT`寄存器，并将定时器使能状态初始化为0。
  - **ticlr_wen**和**tcfg_wen**：当写使能信号有效时，分别处理定时器中断清除和定时器配置的操作。
  - **timer_en**：根据定时器使能状态和定时器值（`CSR_TVAL`）更新异常状态寄存器。
  - **excp_flush**：当发生例外时，更新相关的异常代码和子代码。
  - **estat_wen**：当写使能信号有效时，将输入的写值更新到`CSR_ESTAT`寄存器的相应字段。

### 5. CSR_ERA (异常返回地址寄存器)

```verilog
always @(posedge CLK) begin
    if (Reset) begin
        CSR_ERA <=  32'b0;
    end
    if (excp_flush) begin
        // 触发例外时更新状态
        CSR_ERA <=  excp_PC;
    end
    else if (era_wen) begin
        // 写操作时更新状态
        CSR_ERA <=  csr_wvalue;
    end
end
```

- **功能说明**：
  - **Reset**：在复位时，初始化`CSR_ERA`寄存器。
  - **excp_flush**：当发生例外时，将当前程序计数器（PC）的值写入`CSR_ERA`寄存器。
  - **era_wen**：当写使能信号有效时，将输入的写值更新到`CSR_ERA`寄存器。

### 6. CSR_BADV (地址错误寄存器)

```verilog
always @(posedge CLK) begin
    if (Reset) begin
        CSR_BADV <=  32'b0;
    end
    if (badv_wen) begin
        // 写操作时更新状态
        CSR_BADV <=  csr_wvalue;
    end
    else if (va_error_in) begin
        // 触发地址错误时更新状态
        CSR_BADV <=  bad_va_in;
    end
end
```

- **功能说明**：
  - **Reset**：在复位时，初始化`CSR_BADV`寄存器。
  - **badv_wen**：当写使能信号有效时，将输入的写值更新到`CSR_BADV`寄存器。
  - **va_error_in**：当发生地址错误时

，将错误的虚拟地址写入`CSR_BADV`寄存器。

### 7. CSR_EENTRY (异常入口寄存器)

```verilog
always @(posedge CLK) begin
    if (Reset) begin
        CSR_EENTRY <=  32'b0;
    end
    if (eentry_wen) begin
        // 写操作时更新状态
        CSR_EENTRY <=  csr_wvalue;
    end
end
```

- **功能说明**：
  - **Reset**：在复位时，初始化`CSR_EENTRY`寄存器。
  - **eentry_wen**：当写使能信号有效时，将输入的写值更新到`CSR_EENTRY`寄存器。

### 8. CSR_SAVE0-3 (保存寄存器0-3)

这里的代码片段没有详细展示，但通常会有类似的结构来处理上下文的保存和恢复。每个保存寄存器在异常处理期间用于保存程序的状态。

### 9. CSR_LLBCTL (LLB控制寄存器)

```verilog
always @(posedge CLK) begin
    if (Reset) begin
        CSR_LLBCTL <=  32'b0;
    end
    if (llbctl_wen) begin
        // 写操作时更新状态
        CSR_LLBCTL <=  csr_wvalue;
    end
end
```

- **功能说明**：
  - **Reset**：在复位时，初始化`CSR_LLBCTL`寄存器。
  - **llbctl_wen**：当写使能信号有效时，将输入的写值更新到`CSR_LLBCTL`寄存器。

### 总结

这段Verilog代码实现了处理器中的多个控制和状态寄存器。每个寄存器模块根据时钟信号（`CLK`）同步更新，并在复位信号（`Reset`）时进行初始化。不同的寄存器通过各自的写使能信号（如`crmd_wen`、`prmd_wen`等）接收外部输入的写值，同时也会根据内部条件（如发生例外或定时器中断）更新其状态。这些寄存器在处理器设计中起着管理和维护控制状态的重要作用。









**6.3****复位**

复位将重新处理器核中的所有逻辑，将电路置于确定的状态。这里将给出复位后处理器的状态的定义。

复位后第一条指令的 PC 是 0x1C000000。由于复位撤销后 MMU 一定处于直接地址翻译模式，所以复

位后所取的第一条指令的物理地址也是 0x1C000000。

复位撤销后，处于确定状态的寄存器内容有：

❖ CSR.CRMD 的 PLV=0，IE=0，DA=1，PG=0，DATF=0，DATM=0；

❖ CSR.EUEN 的 FPUen 为 0；

❖ CSR.ECFG 中的 LIE 为 0；

❖ CSR.ESTAT 中 IS[1:0]均为 0；

❖ CSR.TCFG 的 En=0；

❖ CSR.LLBCTL 的 KLO=0；

❖ 

所有实现的 CSR.DMW 中的 PLV0、PLV3 均为 0；

除了上述指定的内容外，复位撤销后，处理器中其它软件可见的寄存器的值都是不确定的，软件在使

用前都要将其状态置于确定状态。

TLB 和 Cache 在复位期间是否进行硬件复位由实现决定，若未实现则需要进行软件复位。





龙芯架构 32 位精简版下的中断采用线中断的形式。每个处理器核内部可记录 12 个线中断，分别是：1

个核间中断（IPI），1 个定时器中断（TI），8 个硬中断（HWI0~HWI7），2 个软中断（SWI0~SWI1）。所有

的线中断都是电平中断，且都是高电平有效。

核间中断的中断输入来自于核外的中断控制器，其被处理器核采样记录在 CSR.ESTAT.IS[12]位。

定时器中断的中断源来自于核内的恒定频率定时器。当恒定频率定时器倒计时至全 0 值时，该中断被

置起。置起后的定时器中断被处理器核采样记录在 CSR.ESTAT.IS[11]位。清除定时器中断需要通过软件向

CSR.TICLR 寄存器的 TI 位写 1 来完成。

硬中断的中断源来自于处理器核外部，其直接来源通常是核外的中断控制器。8 个硬中断 HWI[7:0]被处

理器核采样记录在 CSR.ESTAT.IS[9:2]位。

软中断的中断源来自于处理器核内部，软件通过 CSR 指令对 CSR.ESTAT.IS[1:0]写 1 则置起软中断，写

0 则清除软中断。

中断在 CSR.ESTAT.IS 域中记录的位置的索引值也被称为中断号（Int Number）。SWI0 的中断号等于 0，

SWI1 的中断号等于 1，……，IPI 的中断号等于 12