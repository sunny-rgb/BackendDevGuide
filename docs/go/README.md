# Go

## 1. Go语言简介

Go 是由 **Google** 在 2007 年设计并于 2009 年开源发布的一种 **静态类型、编译型** 编程语言，设计目标是 **高效、简洁、并发友好**。语法类似 C，但更简洁，去除了复杂的特性，自动内存管理（垃圾回收 GC），采用 **Goroutine**（轻量级线程）和 **Channel**（通信机制）实现并发编程，比传统线程更高效。因此Go能适用于Web后台、数据库、区块链等众多场景，Go属于相对较新、发展前景很好的语言，大厂相关的招聘岗位也逐渐增加。

## 2. Go安装与开发环境配置

[Go安装包](https://go.dev/dl/)

**IDE**推荐使用**JetBrains**系列的**[GoLand](https://www.jetbrains.com/go/promo/?source=google&medium=cpc&campaign=APAC_en_ASIA_GoLand_Branded&term=goland&content=546094953593&gad_source=1&gad_campaignid=10165081362&gbraid=0AAAAADloJzjuSZdYd7iq3-ndTIqNwPYeV&gclid=CjwKCAjwmenCBhA4EiwAtVjzmsQy6ms70b_4mvqHL-ceB_EAFpCzPrWGtTC1f6fCNy2Ydg9pjhKL9xoCsKYQAvD_BwE)**，功能非常强大，并且开箱即用，海量插件扩展，生态完善。

## 3. Go语法与并发编程

### 3.1 语法基础

Go语言的语法相比较C++而言，简单一些，可以根据下面的思维导图进行快速学习，重点理解掌握**数组**、**切片**、**Map**以及**指针**的使用。

![](assets/go语言/Go语法基础.svg)

#### 结构体

和C、C++、Java等其它高级语言一样，go也定义了结构体，用于保存对象的多个不同数据类型的信息

结构体示例：

```go
package main

import "fmt"

type Teacher struct {
	ID      int
	Name    string
	Age     int
	Salary  float32
	Subject string
}

func main() {
	teacher := Teacher{
		ID:      10086,
		Name:    "hangman",
		Age:     28,
		Salary:  10000,
		Subject: "math",
	}
    
	fmt.Printf("老师：%v\n", teacher)
	fmt.Printf("老师的工资是：%.2f\n", teacher.Salary)
}
```

上述代码定义了一个Teacher类型的结构体，包含ID、Name、Age、Salary和Subject等属性。对这个结构体进行了初始化，以及访问内部数据的操作。

#### 数组

数组定义和初始化示例：

```go
package main

import "fmt"

func main() {
	// 定义一个长度为 5 的 int 数组，自动初始化为 0
	var array1 [5]int
	fmt.Println("array1:", array1)

	// 定义同时初始化
	var array2 = [5]int{1, 2, 3, 4, 5}
	fmt.Println("array2:", array2)

	// 使用 ... 让编译器推导长度
	array3 := [...]string{"Go", "Python", "Java"}
	fmt.Println("array3:", array3)

	// 部分初始化，未指定的元素默认为零值
	array4 := [5]int{1: 10, 3: 30}
	fmt.Println("array4:", array4) // [0 10 0 30 0]

	// 遍历数组
	for i, v := range array2 {
		fmt.Printf("index: %d, value: %d\n", i, v)
	}

	// 获取数组长度
	fmt.Println("Length of nums3:", len(array3))
}
```

#### 切片

切片定义和初始化示例：

```go
package main

import "fmt"

func main() {
	// 定义一个空切片
	var s1 []int
	fmt.Println("s1:", s1) // []

	// 初始化切片
	s2 := []int{1, 2, 3}
	fmt.Println("s2:", s2)

	// 使用 make 初始化（指定长度和容量）
	s3 := make([]int, 3, 5) // 长度 3，容量 5
	s3[0] = 10
	s3[1] = 20
	s3[2] = 30
	fmt.Println("s3:", s3)

	// 使用 append 动态追加元素
	s3 = append(s3, 40)
	s3 = append(s3, 50, 60) // 超过原容量，会自动扩容
	fmt.Println("s3 after append:", s3)

	// 从数组或切片切片
	arr := [5]int{100, 200, 300, 400, 500}
	s4 := arr[1:4]         // 不包含结束索引
	fmt.Println("s4:", s4) // [200 300 400]

	// 遍历切片
	for i, v := range s2 {
		fmt.Printf("index: %d, value: %d\n", i, v)
	}

	// 获取长度和容量
	fmt.Println("len(s3):", len(s3))
	fmt.Println("cap(s3):", cap(s3))
}
```

其中，切片可以通过make初始化，注意区别长度和容量。以s3为例，初始声明了长度为3，容量为5的切片。长度3是指当前能用的只有 `s3[0]` ~ `s3[2]`，如果你 `append` 两个元素，长度会变成 5，还不会重新分配内存。如果再 `append` 一个（超过 5），Go 会帮你分配一个更大的新底层数组（通常翻倍）。

#### Map

Map定义和初始化示例：

```go
package main

import "fmt"

func main() {
	// 定义一个空 map（key 是 string，value 是 int）
	var m1 map[string]int  // nil map
	fmt.Println("m1:", m1) // 输出：map[]

	// 使用 make 初始化
	m1 = make(map[string]int)
	m1["Alice"] = 90
	m1["Bob"] = 85
	fmt.Println("add m1:", m1)

	// 使用 字面量 初始化
	m2 := map[string]string{
		"apple":  "苹果",
		"banana": "香蕉",
	}
	fmt.Println("m2:", m2)

	// 查找元素，判断 key 是否存在
	score, ok := m1["Alice"]
	fmt.Println("Alice's score:", score, "exists?", ok)

	score2, ok2 := m1["Janny"] // Charlie 不存在
	fmt.Println("Janny:", score2, "exists?", ok2)

	// 删除元素
	delete(m1, "Bob")
	fmt.Println("deleted m1:", m1)

	// 遍历元素（无序）
	for k, v := range m2 {
		fmt.Printf("key: %s, value: %s\n", k, v)
	}

	// 获取长度
	fmt.Println("len(m1):", len(m1))
}
```

注：上述代码片段和文字只是简单介绍一下数组、切片和Map的基本使用，底层数据结构原理将在后续部分说明。

#### 指针

go语言指针定义和基本使用示例：

```go
package main

import "fmt"

func main() {
	// 定义一个 int 变量
	var num = 100
	fmt.Println("num:", num)

	// 定义一个指向 int 的指针，并用 & 取地址初始化
	var p *int
	p = &num
	fmt.Println("指针 p 的值（即 num 的地址）:", p)
	fmt.Println("通过指针 p 访问 num 的值:", *p)

	// 通过指针修改变量的值
	*p = 200
	fmt.Println("修改后 num:", num) // num 变成 200

	// 使用 new 关键字创建指针
	p2 := new(int) // p2 是 *int，new 返回的是指针
	*p2 = 300
	fmt.Println("p2 的值:", *p2)

	// 指针可以为 nil
	var p3 *string
	fmt.Println("未初始化的指针 p3:", p3)
	if p3 == nil {
		fmt.Println("p3 是 nil")
	}

	// 指针数组示例
	nums := [3]int{10, 20, 30}
	ptrs := [3]*int{&nums[0], &nums[1], &nums[2]}
	for i, p := range ptrs {
		fmt.Printf("index: %d, value: %d\n", i, *p)
	}
}
```

#### 条件语句

**if-else语句**

```go
package main

import "fmt"

func main() {
	age := 20

	if age >= 18 {
		fmt.Println("成年人")
	} else if age >= 13 && age < 18 {
		fmt.Println("青少年")
	} else {
		fmt.Println("儿童")
	}

	// if 语句还可以带初始化语句
	if score := 85; score >= 60 {
		fmt.Println("及格")
	} else {
		fmt.Println("不及格")
	}
}
```

**switch-case语句**

```go
package main

import "fmt"

func main() {
	grade := "B"

	switch grade {
	case "A":
		fmt.Println("优秀")
	case "B", "C":
		fmt.Println("良好")
	case "D":
		fmt.Println("及格")
	default:
		fmt.Println("不及格")
	}

	// switch 也可以不带表达式，写类似 if-else 的逻辑
	score := 75
	switch {
	case score >= 90:
		fmt.Println("优秀")
	case score >= 60:
		fmt.Println("及格")
	default:
		fmt.Println("不及格")
	}
}
```

#### 循环语句

go语言的循环不像其它语言一样有多种，比如c++有for，while，do-while。在go语言中循环就只有for一种，上手写起来非常的快。

```go
package main

import "fmt"

func main() {
	// 基本 for 循环
	for i := 0; i < 5; i++ {
		fmt.Println(i)
	}

	// 类似 while 的用法
	i := 0
	for i < 5 {
		fmt.Println(i)
		i++
	}

	// 遍历数组、切片
	arr := []string{"a", "b", "c"}
	for index, value := range arr {
		fmt.Println(index, value)
	}
}
```

#### 函数

```go
package main

import "fmt"

// Add 是一个普通函数，不属于任何类型
func Add(a int, b int) int {
	return a + b
}

func main() {
	// 使用函数
	sum := Add(3, 5)
	fmt.Println("Sum:", sum)
}
```

#### 方法

```go
package main

import "fmt"

// Rectangle 定义一个结构体
type Rectangle struct {
	Width  float64
	Height float64
}

// Area : 给 Rectangle 定义一个方法 Area
// 注意：方法有“接收者” (r Rectangle)
func (r Rectangle) Area() float64 {
	return r.Width * r.Height
}

// Scale : 使用指针接收传来的修改值
func (r *Rectangle) Scale(factor float64) {
	r.Width *= factor
	r.Height *= factor
}

func main() {
	// 使用结构体 + 方法
	rect := Rectangle{Width: 4, Height: 5}
	fmt.Println("Rectangle area:", rect.Area())

	// 调用指针方法，修改值
	rect.Scale(2)
	fmt.Println("Scaled rectangle area:", rect.Area())
}
```

#### 接口

```go
package main

import "fmt"

type Rectangle struct {
	Width  float64
	Height float64
}

type Circle struct {
	Radius float64
}

// Shape : 定义一个接口 Shape
type Shape interface {
	Area() float64
}

// Area : 给 Rectangle 定义一个方法 Area()
func (r Rectangle) Area() float64 {
	return r.Width * r.Height
}

// Area : Circle 也实现了 Area() 方法，所以满足 Shape 接口
func (c Circle) Area() float64 {
	return 3.1415 * c.Radius * c.Radius
}

func main() {
	// 使用接口
	var shape Shape

	rect := Rectangle{Width: 4, Height: 5}
	shape = rect // Rectangle 实现了 Shape 接口
	fmt.Println("Shape area (Rectangle):", shape.Area())

	circle := Circle{Radius: 3}
	shape = circle // Circle 也实现了 Shape 接口
	fmt.Println("Shape area (Circle):", shape.Area())
}
```

### 3.2 语法进阶与并发编程

#### 并发概述

要理解go语言的并发，首先需要理解清楚计算机操作系统中的**进程、线程、协程概念**和**并行与并发的区别**。

#### Goroutine

#### Channel

#### Sync

#### Select

#### Context

#### 定时器

#### 协程池

#### 反射

## 4. Go底层原理

### 4.1 设计模式

### 4.2 程序初始化

### 4.3 数据结构

### 4.4 协程调度

### 4.5 逃逸分析

### 4.6 Go语言垃圾回收

### 4.7 Go内存管理

## 5. 微服务框架与项目实践

### 5.1 go常用微服务框架

[go微服务框架对比](https://zhuanlan.zhihu.com/p/488233067)

### 5.2 kratos

**Kratos** 是由哔哩哔哩（Bilibili）开源的、面向微服务场景的 Go 语言框架，帮助开发者基于 Go 实现高可维护性、高性能和云原生友好的分布式系统。它集成了服务发现、配置管理、熔断、限流、链路追踪、日志等微服务常用能力，同时提供清晰的项目结构和代码生成工具。Kratos 广泛应用于高并发、高可用的互联网业务场景，是 Go 微服务领域较为成熟的工程化实践框架之一。

#### 5.2.1 Kratos架构

![](assets/kratos/Kratos架构.png)

API：HTTP/JSON、GRPC/Protobuf

Error：枚举

DI：依赖注入（类似于Java Spring里面的依赖注入）

Auth：鉴权，流量的入口处，可以去拦截非正常请求

Config：配置相关，和数据源打交道，比如MySQL，Redis

Registry：注册中心，和服务的注册及发现有关

Encoding：内容编码，传输内容的编码格式是什么样子的，比如UTF-8、GBK

Transport：HTTP/GRPC的传输层。

Middleware：中间件中间层，主要起拦截的作用（类似于Java的切面）

Logging：日志相关

Metrics：指标监控，比较主流的是Prometheus。可以用来监控CPU、内存等

Tracing：链路追踪

Database/Cache：数据库和缓存

#### 5.2.2 Kratos项目搭建

**Kratos快速入门应用学习资料**

[Kratos官方文档](https://go-kratos.dev/docs/)

[Kratos简介（视频）](https://www.bilibili.com/video/BV1xq4y1W78N?spm_id_from=333.788.videopod.episodes&vd_source=bf13787311127d9efdb95deea8b81a48)

[Kratos源码（github）](https://github.com/go-kratos/kratos)

下面是比较简短的拉取并启动一个Kratos项目的脚本，更详细的命令和介绍可以阅读上述的Kratos官方文档。

> **启动Go官方依赖管理**
>
> go env -w GO111MODULE=on

> **配置代理（常用于国内用户网络限制）**
>
> go env -w GOPROXY=https://goproxy.cn,direct

> **CLI工具**
>
> go install github.com/go-kratos/kratos/cmd/kratos/v2@latest

>**查看Kratos版本**
>
>kratos --version

上述kratos CLI安装成功之后，会显示当前全局的kratos版本，一般是 v2.xx。需要注意的是，CLI 工具版本和你项目中的框架版本可以不一样

> **通过Kratos CLI创建项目模板**
>
> kratos new helloworld

上述命令行成功执行之后，会出现一个 `helloworld` 项目，首先要做的就是简单了解一下kratos搭建的项目目录结构。

![](assets/kratos/kratos%E9%A1%B9%E7%9B%AE%E7%BB%93%E6%9E%84.png)



> **运行项目**
>
> kratos run

可以看到helloworld项目中默认写好了一个http服务，端口号是8000。启动成功之后，在本地浏览器中输入 `127.0.0.1:8000//helloworld/message`，浏览器能正常显示，就代表项目成功跑起来。接下来根据需求在项目的对应目录添加代码和业务逻辑即可。



#### 5.2.3 Kratos常见问题

##### **Q1. Kratos常用通信协议**

在 Go 的 Kratos 框架中，`http` 和 `grpc` 是两种常见的服务通信协议。

 **HTTP/RESTful API**

- 基于 HTTP/1.1 或 HTTP/2，使用 JSON 数据格式
- 易于使用，适合对外提供开放 API

**gRPC（Google Remote Procedure Call）**

Kratos 默认首推的通信协议。

- 基于 HTTP/2，支持双向流、流控、Header 压缩
- 使用 Protocol Buffers（.proto）定义服务接口和消息格式
- 高性能、强类型，适用于微服务之间的高效通信

- 内置 `proto` 文件编译、服务生成、注册发现等完整支持
- 常用于服务间内部通信

下表是这两种常用服务的对比：

| 特性                  | HTTP（REST）                    | gRPC（RPC）                      |
| --------------------- | ------------------------------- | -------------------------------- |
| **协议**              | HTTP/1.1                        | HTTP/2（支持流式传输）           |
| **接口定义**          | 手动写路由和 handler            | 使用 `.proto` 文件生成代码       |
| **传输格式**          | JSON                            | Protobuf（二进制，更小更快）     |
| **调试便利性**        | 非常方便（浏览器/curl/Postman） | 较麻烦，需要 grpcurl / grpcui 等 |
| **性能**              | 一般                            | 更高效，延迟低                   |
| **服务发现/负载均衡** | 手动或借助外部工具              | 支持内建负载均衡 + 服务发现      |
| **流式通信**          | 不支持                          | 支持双向流（streaming）          |
| **使用场景**          | 对外开放API                     | 微服务间通信                     |

也就是说，当用户直接调用/需要对外提供服务的时候，使用HTTP协议；微服务之间调用/内部服务的时候，使用gRPC协议，常用于对性能要求高，数据量大的时候。

Q2. 

### 5.3 活动抽奖系统

### 5.4 线上聊天论坛app

基于**Go + Kratos + Gen + Grom**实现线上聊天论坛app/小程序/网页端

[Kratos快速入门搭建项目（视频）](https://www.bilibili.com/video/BV1t3411h7uA/?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click&vd_source=bf13787311127d9efdb95deea8b81a48)

[Kratos快速入门项目源码（github）](https://github.com/gothinkster/realworld)

[TangSengDaoDaoServer源码（高颜值 IM 即时通讯,聊天）](https://github.com/TangSengDaoDao/TangSengDaoDaoServer)



## 6. 面试题库







