

![image-20240718141837086](D:\blog\计算机组成原理\龙芯架构\assets\image-20240718141837086-1721283521077-17.png)



## tlb

![image-20240718141455115](D:\blog\计算机组成原理\龙芯架构\assets\image-20240718141455115-1721283297667-1-1721283299120-3-1721283300415-5-1721283301207-7-1721283302538-9-1721283303167-11-1721283303727-13-1721283307536-15.png)

系统软件通过配置 CSR.DMW0~CSR.DMW1 寄存器来分别设置两个直接映射配置窗口。每个窗口除了

地址范围信息外，还可以配置该窗口在哪些特权等级下可用，以及虚地址落在该窗口上的访存操作的存储

访问类型

```verilog
//dmw0
always @(posedge CLK) begin
    if (Reset) begin
//        csr_dmw0[ 2:1] <= 2'b0;
//        csr_dmw0[24:6] <= 19'b0;
//        csr_dmw0[28]   <= 1'b0;
          csr_dmw0 <= 32'b0;
    end
    else if (DMW0_wen) begin
        csr_dmw0[`PLV0]    <= csr_wvalue[`PLV0];
        csr_dmw0[`PLV3]    <= csr_wvalue[`PLV3];
        csr_dmw0[`DMW_MAT] <= csr_wvalue[`DMW_MAT];
        csr_dmw0[`PSEG]    <= csr_wvalue[`PSEG];
        csr_dmw0[`VSEG]    <= csr_wvalue[`VSEG];
    end
end

//dmw1
always @(posedge CLK) begin
    if (Reset) begin
//        csr_dmw1[ 2:1] <= 2'b0;
//        csr_dmw1[24:6] <= 19'b0;
//        csr_dmw1[28]   <= 1'b0;
            csr_dmw1 <= 32'b0;
    end
    else if (DMW1_wen) begin
        csr_dmw1[`PLV0]    <= csr_wvalue[`PLV0];
        csr_dmw1[`PLV3]    <= csr_wvalue[`PLV3];
        csr_dmw1[`DMW_MAT] <= csr_wvalue[`DMW_MAT];
        csr_dmw1[`PSEG]    <= csr_wvalue[`PSEG];
        csr_dmw1[`VSEG]    <= csr_wvalue[`VSEG];
    end
end
```

