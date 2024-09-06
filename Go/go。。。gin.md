```go
package main

import (
    "github.com/gin-gonic/gin"
    "net/http"
)

func main() {
    //初始化路由
    router := gin.Default()
    //加载templates中所有模板文件, 使用不同目录下名称相同的模板,注意:一定要放在配置路由之前才得行
    router.LoadHTMLGlob("templates/*")
    //router.LoadHTMLFiles("templates/template1.html", "templates/template2.html")
    router.SetTrustedProxies([]string{"}127.0.0.1"})
    router.GET("/", func(c *gin.Context) {
       c.HTML(http.StatusOK, "index.html", gin.H{
          "title": "首页",
       })
    })

    router.GET("/aaa", func(c *gin.Context) {
       c.HTML(http.StatusOK, "index.html", map[string]interface{}{
          "title": "前台首页",
       })
    })
    router.Run(":8080")
}
```