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





## 变量

**变量的申明**：

声明变量的一般形式是使用 **var 关键字**

例如：

```go
/*
四种变量的声明方式
*/
package main

import "fmt"

/*
	声明全局变量时，方法一、二、三没有区别 都是可以的
	但是方法四的(:=)不支持声明全局变量
*/
var glob_a int = 50
var glob_b int
var glob_c string = "iWyh2"


func main() {
	//方法一: 声明一个变量 默认值为0
	var a int
	fmt.Println("a = ", a)

	//默认值为空
	var as string
	fmt.Println("as = ", as)

	//方法二: 声明一个变量 声明的同时进行初始化
	var b int = 50
	fmt.Println("b = ", b)

	//方法三: 声明一个变量并对其进行初始化时 省去变量的数据类型 通过初始化值自动匹配当前变量的数据类型
	var c = 100
	fmt.Printf("type of c is %T\n", c)//格式化输出 输出当前变量的数据类型用%T
	fmt.Println("c = ", c)

	//方法四: 省去var关键字 直接自动匹配进行声明并初始化赋值 - 常用这种方法 (但不支持声明全局变量 只适合在方法体内进行使用)
	d := 100
	fmt.Printf("type of d is %T\n", d)//格式化输出 输出当前变量的数据类型用%T
	fmt.Println("d = ", d)

	//输出全局变量
	fmt.Printf("全局变量a, b, c为: %d %d %s\n", glob_a, glob_b, glob_c)

	//多变量声明 - 方式一: 一行书写
	var x, y int = 10, 20//相同类型
	var z, w = 0, "wang"//不同类型 自动匹配
	fmt.Println("x = ", x, "y = ", y)//可以多字符串拼接式输出
	fmt.Println("z = ", z, "w = ", w)

	//多变量声明 - 方式二: 多行书写 (一般适用于多且 不同变量类型 时) 也可以适用于全局变量的书写
	var (
		xx int
		yy bool = false
		zz string = ""
	)
	fmt.Println("xx = ", xx,"yy = ", yy,"zz = ", zz)
}
```



* 第一种，指定变量类型，声明后若不赋值，使用默认值
* 第二种，根据值自行判定变量类型
* 第三种，省略var, 注意 :=左侧的变量不应该是已经声明过的，否则会导致编译错误，可以叫 -> 声明初始化





## 常量

常量是一个简单值的标识符，在程序运行时，不会被修改的量。

常量中的数据类型只可以是：

* 布尔型
* 数字型（整数型、浮点型和复数）
* 字符串型

常量的定义格式：

```go
const identifier [type] = value
```

【注】可以省略类型说明符 [type]，因为编译器可以根据变量的值来推断其类型



const还可以定义**枚举**，也就是多个常量的定义。**枚举必须赋值**

```go
const(
	Spring = 1
	Summer = 2
	Autumn = 3
	Winter = 4
)
```

常量可以用len(), cap(), unsafe.Sizeof()常量计算表达式的值

：常量表达式中，函数必须是内置函数，否则编译不过

```go
import "unsafe"
const (
    a = "abc"
    b = len(a)
    c = unsafe.Sizeof(a)
)
```

【注】**字符串**类型在 go 里**是个结构**, 包含指向底层数组的**指针和长度**,这两部分每部分**都是 8 个字节**，所以**字符串类型大小为 16 个字节**



**iota关键字**只用于const()中



例子：

```go
package main

import "fmt"

/*
	const可以定义枚举类型 (枚举必须赋值) : 也就是多个常量定义
	
	在定义枚举时 可以使用关键字: iota
	第一行的iota值默认为0 iota在每一行都会累加1
	iota只能在const()中使用
*/
const(
	Spring = iota//此时 Spring = 0
	Summer//此时 Summer = 1
	Autumn//此时 Autumn = 2
	Winter//此时 Winter = 3
)

/*
	多个常量可以放在同一行
	枚举第一行的iota值默认为0
	iota还可以用在表达式中
*/
const(
	dog, cat = iota * 10, iota * 20//此时 dog = 0  cat = 0
	pig, kit//此时 pig = 10 kit = 20
	tgr, lon = iota * 10 + 5, iota * 20 + 10//此时 tgr = 25  lon = 50
)

func main() {
	//常量定义
	const length int = 10//只读
	//length = 20 is not allowed :-> 常量是不允许再被修改的

	//打印常量
	fmt.Println("Spring = ", Spring)
	fmt.Println("Summer = ", Summer)
	fmt.Println("Autumn = ", Autumn)
	fmt.Println("Winter = ", Winter)
	//打印常量
	fmt.Println("dog = ", dog, "cat = ", cat)
	fmt.Println("pig = ", pig, "kit = ", kit)
	fmt.Println("tgr = ", tgr, "lon = ", lon)
}
```





## 函数

### 多返回值

话不多说，直接上例子：

```go
package main

import "fmt"

/*
	Go的函数 参数类型放在变量名之后
	返回值放在 ）与 { 之间
*/

//单个返回值
func func1(a string, b int) int {
	fmt.Println("a = ", a, "b = ", b)
	c := 20
	return c
}

/*
	多返回值 - 用括号包裹起来
	且还可以给返回值取名 未取名为匿名返回

	取名的多返回值可以看成是函数的形参，在定义时就有了初始化值，比如int为0等
	（Go的变量在定义时都有初始化值，防止野指针的出现）
*/
func func2(a int, b bool) (string, string) {
	fmt.Println("a = ", a, "b = ", b)
	return "iWyh2","20"
}

func func3(a int, b bool) (name string, age string) {
	fmt.Println("a = ", a, "b = ", b)

	//给有名称的返回值赋值
	name = "iWyh2"
	age = "20"

	//直接返回就无须再手动返回值，由带名称的返回值直接返回
	return
}

//多返回值类型相同时，类型可以放一起写
func func4(a int, b bool) (name , age string) {
	fmt.Println("a = ", a, "b = ", b)

	//给有名称的返回值赋值
	name = "iWyh2"
	age = "20"

	//直接返回就无须再手动返回值，由带名称的返回值直接返回
	return
}

func main() {
	a := func1("wyh", 20)
	fmt.Println("main's a = ", a)

	name, age := func2(20,true)
	fmt.Println("name = ", name, "age = ", age)

	name, age = func3(20,true)
	fmt.Println("name = ", name, "age = ", age)

	name, age = func4(20,true)
	fmt.Println("name = ", name, "age = ", age)
}
```





### init函数

main 函数只能在package main中

**init 函数**可在package main中，可在其他package中，**可在同一个package中出现多次**

**执行顺序**：

golang里面有两个保留的函数：init函数（能够应用于所有的package）和main函数（只能应用于package main）。**这两个函数在定义时不能有任何的参数和返回值**

虽然一个package里面可以写任意多个init函数，但这无论是对于可读性还是以后的可维护性来说，我们都强烈建议用户**在一个package中每个文件只写一个init函数**

**go程序会自动调用init()和main()**，所以你不需要在任何地方调用这两个函数。每个package中的init函数都是可选的，但**package main就必须包含一个main函数**。

**程序的初始化和执行都起始于main包**

如果**main包还导入了其它的包，那么就会在编译时将它们依次导入**。有时一个包会被多个包同时导入，那么它只会被导入一次（例如很多包可能都会用到fmt包，但它只会被导入一次，因为没有必要导入多次）。

当一个包被导入时，如果该包还导入了其它的包，那么会先将其它包导入进来，然后再对这些包中的包级常量和变量进行初始化，接着执行init函数（如果有的话），依次类推。

等所有被导入的包都加载完毕了，就会开始对main包中的包级常量和变量进行初始化，然后执行main包中的init函数（如果存在的话），最后执行main函数。下图详细地解释了整个执行过程：

![31-init.png](https://cdn.nlark.com/yuque/0/2022/png/26269664/1650528765014-63d3d631-428e-4468-bc95-40206d8cd252.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0)



例子：

例子结构：![image-20221228181129868](.\images\init.png)

lib1包：lib1.go

```go
package lib1 //一个包对应一个文件夹（除了main包）

import "fmt"

//lib1中的API定义
/*
	go的函数的定义
	函数名首字母大写为对外可访问 相当于 - public
	首字母小写为对外不可访问 只有当前包内可调用 - private
*/
func Lib1TestAPI() {
	fmt.Println("lib1's func")
}


func init() {
	fmt.Println("lib1's initFunc is running...")
}
```

lib2包：lib2.go

```go
package lib2

import "fmt"

//lib2中的API定义
func Lib2TestAPI() {
	fmt.Println("lib2's func")
}


func init() {
	fmt.Println("lib2's initFunc is running...")
}
```

main包：main.go

```go
package main

import (
	"GoStudy/Go-5-init/lib1" //需要写完整gopath的相对路径 不然会找不到包 之后会用mod解决 注意还要关闭go-module
	"GoStudy/Go-5-init/lib2" //建议文件名不要带中文 会报奇奇怪怪的错
)

func main() {
	lib1.Lib1TestAPI()
	lib2.Lib2TestAPI()
}
```

【注】Go的函数的定义：要注意区分函数名首字母的大小写（这是包访问权限的设置）





### import的使用方式

直接看例子：

```go
package main

import (
	_ "GoStudy/Go-5-init/lib1"   //导入包并起别名 此处为匿名 - 也就是导入包使用包的init方法 但不使用当前包内的方法函数
	wyh "GoStudy/Go-5-init/lib2" //导入包并起别名 此处为起别名 - 也就是正常导入并使用 只是用别名来调用了
	//. "GoStudy/Go-5-init/lib2" //使导入的包可以直接调用API 而不是使用包名调用（但一般情况下不建议这样使用，防止两个包有同名函数）
)



func main() {
	//lib1.Lib1TestAPI() 被匿名导入 无法调用包内的函数
	wyh.Lib2TestAPI()
}
```

* 匿名导入包（导入但不使用 防止报错）
* 别名导入包（导入后用别名调用）
* 直接导入包（正常使用）





## 指针

学过C就会

Go的指针定义:

```go
a := 2
var p *int = &a
fmt.Print(p)//a的内存地址
fmt.Print(*p)//a的实际的值
```



例子：

```go
package main

import (
	"fmt"
)

/*
	快速掌握指针的小例子 - 交换两个数据的值
*/

//按值传递
func exchange(a int, b int) {
   temp := a
   a = b
   b = temp
}

//按址传递
func swap(pa *int, pb *int) {
	temp := *pa
	*pa = *pb
	*pb = temp
}

func main() {
	value1 := 10
	value2 := 100

	exchange(value1,value2)//没有成功交换值
	fmt.Println("After exchange: value1 = ",value1,"value2 = ",value2)

	swap(&value1, &value2)
	fmt.Println("After swap: value1 = ",value1,"value2 = ",value2)

	var p *int = &value1
	fmt.Println("p = ",p,"*p = ",*p)
	pp := &p//二级指针
	fmt.Println("pp = ",pp,"*pp = ",*pp,"**pp = ",*(*pp))
}
```





## defer





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