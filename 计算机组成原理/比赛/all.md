```verilog
    assign flush_sign_IF = excp_flush || ertn_flush || refetch_flush || icacop_flush || idle_flush;//kqy
    assign flush_sign_WB = excp_flush || ertn_flush || refetch_flush || icacop_flush || idle_flush;//kqy
	assign flush_sign_EX = excp_flush || ertn_flush || refetch_flush || icacop_flush || idle_flush;//kqy
	assign flush_sign_MEM = excp_flush || ertn_flush || refetch_flush || icacop_flush || idle_flush;//kqy
    assign flush_sign_ID = excp_flush || ertn_flush || refetch_flush || icacop_flush || idle_flush;//kqy


    assign excp_flush = excp_WB && valid_WB;
    assign ertn_flush = ertn_WB && valid_WB;

 assign icacop_flush = icacop_op_en_WB && valid_WB;
```



