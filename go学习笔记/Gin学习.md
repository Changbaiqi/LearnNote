---
title: Gin学习
date: 2025-04-03 21:53:00
author: 长白崎
categories:
  - "GO"
tags:
  - "GO"
  - "Gin"
---

# Gin学习

---





## 一、Gin导入与基础使用预览

### 1、安装Gin框架

```shell
go get -u github.com/gin-gonic/gin
```

Gin 使用 `encoding/json` 作为默认的 json 包，但是你可以在编译中使用标签将其修改为 [jsoniter](https://github.com/json-iterator/go)。

```shell
$ go build -tags=jsoniter .
```



### 2、创建第一个Gin应用

```go
package main

import "github.com/gin-gonic/gin"

func main() {
    // 新建一个没有任何默认中间件的路由
	r := gin.New()
    r.GET("/", func(c *gin.Context) {
        c.String(200, "Hello, Gin!")
    })
    r.Run(":8080")
}
```

在上面的代码中，我们首先导入了Gin框架的包，然后创建了一个默认的Gin引擎，并定义了一个路由，最后启动了Gin应用程序。



### 3、常用功能

除了基本的路由功能外，Gin框架还提供了许多常用的功能，如中间件、参数解析、日志记录等。下面是一个使用中间件和参数解析的示例：

```go
package main

import (
    "github.com/gin-gonic/gin"
    "net/http"
)

func main() {
    // 新建一个没有任何默认中间件的路由
	r := gin.New()
    
    
	// 全局中间件
	// Logger 中间件将日志写入 gin.DefaultWriter，即使你将 GIN_MODE 设置为 release。
	// By default gin.DefaultWriter = os.Stdout
    r.Use(gin.Logger())
    // Recovery 中间件会 recover 任何 panic。如果有 panic 的话，会写入 500。
    r.Use(gin.Recovery())

    r.GET("/hello", func(c *gin.Context) {
        name := c.Query("name")
        c.JSON(http.StatusOK, gin.H{"message": "Hello, " + name})
    })

    r.Run(":8080")
}
```

在上面的示例中，我们首先使用了Logger和Recovery中间件，然后定义了一个带有参数解析的路由，最后启动了Gin应用程序。



### 4、路由定义和处理：

```shell
package main

import (
	"github.com/gin-gonic/gin"
)

func main() {
	router := gin.Default()

	// GET 请求处理
	router.GET("/hello", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "Hello, World!",
		})
	})

	// POST 请求处理
	router.POST("/users", func(c *gin.Context) {
		var user User
		if err := c.ShouldBindJSON(&user); err != nil {
			c.JSON(400, gin.H{
				"error": err.Error(),
			})
			return
		}

		// 处理接收到的用户数据
		// ...

		c.JSON(200, gin.H{
			"message": "User created successfully",
		})
	})

	router.Run(":8080")
}
```



### 5、参数化路由和路由组：

```go
package main

import (
	"github.com/gin-gonic/gin"
)

func main() {
	router := gin.Default()

	// 参数化路由
	router.GET("/users/:id", func(c *gin.Context) {
		id := c.Param("id")
		c.String(200, "User ID: %s", id)
	})

	// 路由组
	v1 := router.Group("/api/v1")
	{
		v1.GET("/users", func(c *gin.Context) {
			c.String(200, "List of users")
		})
		v1.POST("/users", func(c *gin.Context) {
			c.String(200, "Create a user")
		})
		v1.PUT("/users/:id", func(c *gin.Context) {
			id := c.Param("id")
			c.String(200, "Update user with ID: %s", id)
		})
		v1.DELETE("/users/:id", func(c *gin.Context) {
			id := c.Param("id")
			c.String(200, "Delete user with ID: %s", id)
		})
	}

	router.Run(":8080")
}
```

这些代码示例展示了如何使用 Gin 框架定义路由和处理请求，包括 GET 和 POST 请求的处理、参数化路由以及路由组的使用。



### 6、模板渲染和静态文件

##### 1. 模板渲染：

Gin 框架内置了对多种模板引擎的支持，包括 HTML 模板引擎、Ace 模板引擎等。你可以通过 `gin.Default()` 方法创建一个默认的路由组，并使用 `LoadHTMLGlob` 或者 `LoadHTMLFiles()`方法来加载模板文件。以下是一个简单的示例：

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	router := gin.Default()

	// 加载模板文件
	router.LoadHTMLGlob("templates/*")

	// 定义路由处理函数，渲染模板
	router.GET("/hello", func(c *gin.Context) {
		c.HTML(http.StatusOK, "hello.tmpl", gin.H{
			"title": "Hello, Gin!",
		})
	})

	router.Run(":8080")
}
```

templates/index.tmpl：

```html
<html>
	<h1>
		{{ .title }}
	</h1>
</html>
```

在这个示例中，我们首先使用 `LoadHTMLGlob` 方法加载了位于 "templates" 目录下的所有模板文件。然后，在 "/hello" 路由处理函数中，我们使用 `c.HTML` 方法渲染了名为 "hello.tmpl" 的模板，并传递了一个包含标题信息的数据。





使用不同目录下名称相同的模板:

```go
func main() {
	router := gin.Default()
	router.LoadHTMLGlob("templates/**/*")
	router.GET("/posts/index", func(c *gin.Context) {
		c.HTML(http.StatusOK, "posts/index.tmpl", gin.H{
			"title": "Posts",
		})
	})
	router.GET("/users/index", func(c *gin.Context) {
		c.HTML(http.StatusOK, "users/index.tmpl", gin.H{
			"title": "Users",
		})
	})
	router.Run(":8080")
}
```

templates/posts/index.tmpl

```html
{{ define "posts/index.tmpl" }}
<html><h1>
	{{ .title }}
</h1>
<p>Using posts/index.tmpl</p>
</html>
{{ end }}
```

templates/users/index.tmpl

```html
{{ define "users/index.tmpl" }}
<html><h1>
	{{ .title }}
</h1>
<p>Using users/index.tmpl</p>
</html>
{{ end }}
```

##### 2. 自定义模板渲染器

可以使用自定义的 html 模板渲染

```go
import "html/template"

func main() {
	router := gin.Default()
	html := template.Must(template.ParseFiles("file1", "file2"))
	router.SetHTMLTemplate(html)
	router.Run(":8080")
}
```

##### 3. 自定义分隔符

你可以使用自定义分隔

```go
	router := gin.Default()
	router.Delims("{[{", "}]}")
	router.LoadHTMLGlob("/path/to/templates")
```

##### 4. 自定义模板功能

查看详细[示例代码](https://github.com/gin-gonic/examples/tree/master/template)。

main.go

```go
import (
    "fmt"
    "html/template"
    "net/http"
    "time"

    "github.com/gin-gonic/gin"
)

func formatAsDate(t time.Time) string {
    year, month, day := t.Date()
    return fmt.Sprintf("%d/%02d/%02d", year, month, day)
}

func main() {
    router := gin.Default()
    router.Delims("{[{", "}]}")
    router.SetFuncMap(template.FuncMap{
        "formatAsDate": formatAsDate,
    })
    router.LoadHTMLFiles("./testdata/template/raw.tmpl")

    router.GET("/raw", func(c *gin.Context) {
        c.HTML(http.StatusOK, "raw.tmpl", map[string]interface{}{
            "now": time.Date(2017, 07, 01, 0, 0, 0, 0, time.UTC),
        })
    })

    router.Run(":8080")
}
```

raw.tmpl

```sh
Date: {[{.now | formatAsDate}]}
```

结果：

```sh
Date: 2017/07/01
```



##### 5. 静态文件服务：

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	router := gin.Default()

	// 从相对路径 "assets" 提供静态文件
	router.Static("/static", "./assets")

	// 从绝对路径 "/tmp" 提供静态文件
	router.StaticFS("/static2", http.Dir("/tmp"))

	// 提供单个静态文件
	router.StaticFile("/favicon.ico", "./resources/favicon.ico")

	router.Run(":8080")
}
```

这个示例展示了如何在 Gin 框架中提供静态文件服务，可以方便地将静态资源文件（如图片、样式表、脚本等）提供给客户端。





### 7、 JSON 解析与绑定：

```go
package main

import (
	"github.com/gin-gonic/gin"
)

type User struct {
	Username string `json:"username"`
	Password string `json:"password"`
}

func main() {
	router := gin.Default()

	router.POST("/login", func(c *gin.Context) {
		var user User
		if err := c.ShouldBindJSON(&user); err != nil {
			c.JSON(400, gin.H{"error": err.Error()})
			return
		}

		// 根据用户输入的用户名和密码进行验证
		// ...

		c.JSON(200, gin.H{"message": "Login successful"})
	})

	router.Run(":8080")
}
```

这个示例演示了如何接收 JSON 格式的请求体，并将其绑定到结构体中进行处理。

这些代码示例展示了 Gin 框架中各种功能的具体使用方法，包括中间件、JSON 解析与绑定等。



### 8、错误处理和日志记录

#### 1. 自定义错误处理函数

Gin 框架允许你注册全局的中间件来处理错误。你可以创建一个中间件函数来捕获处理程序中的错误，并返回自定义的错误响应。以下是一个简单的示例：

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	router := gin.Default()

	// 自定义全局中间件处理错误
	router.Use(func(c *gin.Context) {
		c.Next()

		// 检查是否有发生错误
		if len(c.Errors) > 0 {
			// 自定义错误处理
			c.JSON(http.StatusInternalServerError, gin.H{"error": "服务器内部错误"})
		}
	})

	router.GET("/ping", func(c *gin.Context) {
		// 模拟处理过程中发生错误
		c.Error(gin.Error{Err: errors.New("处理过程中发生错误")})
	})

	router.Run(":8080")
}
```

在这个示例中，我们创建了一个全局中间件函数来检查处理过程中是否有错误发生，如果有错误则返回自定义的错误响应。在路由处理函数中，我们通过 `c.Error` 方法模拟了一个处理过程中发生的错误。



##### 2. 使用 Gin 框架的日志功能

Gin 框架默认集成了日志功能，你可以直接使用 `gin.Default()` 方法创建的默认路由组来记录日志。以下是一个示例：

```go
package main

import (
	"github.com/gin-gonic/gin"
	"os"
)

func main() {
	// 将日志输出到文件
	f, _ := os.Create("gin.log")
	gin.DefaultWriter = io.MultiWriter(f, os.Stdout)

	router := gin.Default()

	router.GET("/ping", func(c *gin.Context) {
		c.String(http.StatusOK, "pong")
	})

	router.Run(":8080")
}
```

在这个示例中，我们将日志输出到文件 "gin.log" 中，并使用 `io.MultiWriter` 来同时输出到文件和标准输出。Gin 框架会自动记录请求的详细信息以及处理时间等日志内容。



### 9、设置当前环境

Gin 项目的部署可以通过环境变量或直接在代码中进行配置。

以下环境变量可用于配置 Gin：

| 环境变量 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| PORT     | 使用 `router.Run()` 启动 Gin 服务器时（即不带任何参数）监听的 TCP 端口。 |
| GIN_MODE | 可设置为 `debug`、`release` 或 `test`。用于管理 Gin 的运行模式，如是否输出调试信息。也可以在代码中使用 `gin.SetMode(gin.ReleaseMode)` 或 `gin.SetMode(gin.TestMode)` 来设置。 |

以下代码可用于配置 Gin：

```go
// 不指定 Gin 的绑定地址和端口。默认绑定所有接口，端口为 8080。
// 使用不带参数的 `Run()` 时，可通过 `PORT` 环境变量更改监听端口。
router := gin.Default()
router.Run()

// 指定 Gin 的绑定地址和端口。
router := gin.Default()
router.Run("192.168.1.100:8080")

// 仅指定监听端口。将绑定所有接口。
router := gin.Default()
router.Run(":8080")

// 设置哪些 IP 地址或 CIDR 被视为可信任的代理，用于设置记录真实客户端 IP 的请求头。
// 更多详情请参阅文档。
router := gin.Default()
router.SetTrustedProxies([]string{"192.168.1.2"})

```



### 10、不要信任所有代理

Gin 允许你指定哪些请求头可以保存真实的客户端 IP（如果有的话），以及你信任哪些代理（或直接客户端）可以设置这些请求头。

在你的 `gin.Engine` 上使用 `SetTrustedProxies()` 函数来指定可信任的网络地址或网络 CIDR，这些地址的请求头中与客户端 IP 相关的信息将被信任。它们可以是 IPv4 地址、IPv4 CIDR、IPv6 地址或 IPv6 CIDR。

**注意：** 如果你没有使用上述函数指定可信代理，Gin 默认会信任所有代理，这**并不安全**。同时，如果你不使用任何代理，可以通过 `Engine.SetTrustedProxies(nil)` 来禁用此功能，这样 `Context.ClientIP()` 将直接返回远程地址，避免不必要的计算。

```go
import (
  "fmt"

  "github.com/gin-gonic/gin"
)

func main() {
  router := gin.Default()
  router.SetTrustedProxies([]string{"192.168.1.2"})

  router.GET("/", func(c *gin.Context) {
    // 如果客户端是 192.168.1.2，则使用 X-Forwarded-For
    // 请求头中可信部分推断出原始客户端 IP。
    // 否则，直接返回客户端 IP
    fmt.Printf("ClientIP: %s\n", c.ClientIP())
  })
  router.Run()
}
```

**提示：** 如果你使用 CDN 服务，可以设置 `Engine.TrustedPlatform` 来跳过 TrustedProxies 检查，它的优先级高于 TrustedProxies。 请看下面的示例：

```go
import (
  "fmt"

  "github.com/gin-gonic/gin"
)

func main() {
  router := gin.Default()
  // 使用预定义的 gin.PlatformXXX 头
  // Google App Engine
  router.TrustedPlatform = gin.PlatformGoogleAppEngine
  // Cloudflare
  router.TrustedPlatform = gin.PlatformCloudflare
  // Fly.io
  router.TrustedPlatform = gin.PlatformFlyIO
  // 或者，你可以设置自己的可信请求头。但要确保你的 CDN
  // 能防止用户传递此请求头！例如，如果你的 CDN 将客户端
  // IP 放在 X-CDN-Client-IP 中：
  router.TrustedPlatform = "X-CDN-Client-IP"

  router.GET("/", func(c *gin.Context) {
    // 如果设置了 TrustedPlatform，ClientIP() 将解析
    // 对应的请求头并直接返回 IP
    fmt.Printf("ClientIP: %s\n", c.ClientIP())
  })
  router.Run()
}
```