# Java后端极速转Go



# Golang环境安装



Go新版本安装：Windows/Linux

GOROOT

GOPATH：工作环境目录

* bin：编译后的可执行文件
* pkg：一些库
* src：项目源码，一个文件一个项目

IDE：VSCode/GoLand







# Golang语言特性



## 优势

![3-golang优势1.png](https://cdn.nlark.com/yuque/0/2022/png/26269664/1650470888012-e20eedfd-9064-4d4e-b040-d6878aaa96ad.png)

![7-golang优势2.png](https://cdn.nlark.com/yuque/0/2022/png/26269664/1650471318257-8884275c-9fe9-41de-8251-1bb828d50aa6.png)

![5-golan优势1.png](https://cdn.nlark.com/yuque/0/2022/png/26269664/1650471363769-9ded1e7a-acb0-4d6c-b4c1-61f9b77589b0.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0)

![9-golang优势4.png](https://cdn.nlark.com/yuque/0/2022/png/26269664/1650471446832-5722e0a9-5522-469b-9ea9-296c373e3d66.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0)

![10-golang优势5.png](https://cdn.nlark.com/yuque/0/2022/png/26269664/1650471454878-bf9c4abc-62c5-42f8-b595-b16d99d10743.png)

![11-golang优势6.png](https://cdn.nlark.com/yuque/0/2022/png/26269664/1650471465058-b5db8451-e1d8-4ce4-a572-cc3d8be9bdc1.png?x-oss-process=image%2Fresize%2Cw_775%2Climit_0)

![12-golang优势7.png](https://cdn.nlark.com/yuque/0/2022/png/26269664/1650471474504-6de5ec53-2447-4bdd-8a08-36ae40283ece.png?x-oss-process=image%2Fresize%2Cw_894%2Climit_0)



## Golang适合做什么



**(1)、云计算基础设施领域**

代表项目：docker、kubernetes、etcd、consul、cloudflare CDN、七牛云存储等。



**(2)、基础后端软件**

代表项目：tidb、influxdb、cockroachdb等。



**(3)、微服务**

代表项目：go-kit、micro、monzo bank的typhon、bilibili等。



**(4)、互联网基础设施**

代表项目：以太坊、hyperledger等



## Golang代表作

![13-golang优势8.png](https://cdn.nlark.com/yuque/0/2022/png/26269664/1650471498432-166e36fd-6294-460c-bbcd-96f6e784f8a9.png?x-oss-process=image%2Fresize%2Cw_730%2Climit_0)

![14-golang优势9.png](https://cdn.nlark.com/yuque/0/2022/png/26269664/1650471506905-c3bf704e-d2fc-41e1-8e01-0a4907ae28fc.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0)



## 不足

* 包管理，大部分包都在**github**上，不安全，不稳定

* 所有**Excepiton**都用**Error**来处理(比较有争议)

* 对**C**的降级处理，并非无缝，没有**C**降级到**asm**那么完美(序列化问题)







# Golang新奇语法



## 语法入门



go run 表示 直接编译go语言并执行应用程序，一步完成

也可以先编译（go build），然后再执行



入门程序：hello.go

```go
package main //表示该文件所在的包为main

// import "fmt" //引入了一个包为fmt，引入了之后才可以使用这个包里面的函数：Println
// import "time"

import ( //导入多个包
	"fmt"
	"time"
)

//func表示是一个函数；main是主函数，程序的入口
func main () {//函数的{必须和函数名在同一行
	fmt.Println("Hello Go, Change The World!")//不加分号

	time.Sleep(1 * time.Second)
}
```

- **package main**定义了包名。你必须在源文件中非注释的第一行指明这个文件属于哪个包，如：package main。package main表示一个可独立执行的程序，每个 Go 应用程序都包含一个名为 main 的包
- **import "fmt"**告诉 Go 编译器这个程序需要使用 fmt 包（的函数，或其他元素），fmt 包实现了格式化 IO（输入/输出）的函数
- func main()是程序开始执行的函数。main 函数是每一个可执行程序所必须包含的，一般来说都是在启动后第一个执行的函数（如果有 **init() 函数**则会先执行该函数）
-  // 是注释，在程序执行时将被忽略。单行注释是最常见的注释形式，你可以在任何地方使用以 // 开头的单行注释。多行注释也叫块注释，均已以 / \*开头，并以* / 结尾，且不可以嵌套使用，多行注释一般用于包的文档描述或注释成块的代码片段
- fmt.Println(...)可以将字符串输出到控制台，并在最后自动增加换行字符 \n。 使用 fmt.Print("hello, world\n") 可以得到相同的结果。 Print 和 Println 这两个函数也支持使用变量，如：fmt.Println(arr)。如果没有特别指定，它们会以默认的打印格式将变量 arr 输出到控制台





## 函数



### 多返回值



### main函数和init函数





## defer





## 指针





## slice(切片)





## map





## interface



### 万能类型



### 类型断言





## 结构体





## 反射





## 面向对象



### 封装



### 继承



### 多态





# GoLang高阶



## Goroutine





## channel





# Go modules(模块管理)



# 经典案例