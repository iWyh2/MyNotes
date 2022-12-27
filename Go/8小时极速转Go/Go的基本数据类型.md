# Go的基本数据类型

## 变量声明及初始化

Golang 推荐使用驼峰式命名，如 QuoteRuneToASCII 和 parseRequestLine 等。

定义变量的几种形式：

```go
// 第一种，先声明类型，再进行初始化赋值
// 如果没有初始化，则变量默认为零值。
var a int
a = 3

// 第二种, 根据赋值自动适配类型
b := 3   // 这种写法只能在函数体中出现
         // 效果等价于 var b = 3， 但后者可以在函数体之外的地方使用

// 第三种，声明类型的同时进行赋值
var c int = 3

// 第四种，使用 new 创建变量，然后返回变量的地址，类型为 *type
p := new(int)
*p = 3

// 交换两个变量的值，可以使用 a, b = b, a 
// 两个变量的类型必须是相同的。
```

注意：

* Golang 中不允许出现未使用的变量，否则会报错 a declared and not used。但是全局变量是允许声明但不使用。
* Golang 的封装性由变量名第一个字母决定，大写字母开头的变量是可导出的变量，可以在包外使用，小写字母或者下划线开头的变量只能在本包内使用。
* “:=” 是一个变量声明语句，而 “=‘ 是一个变量赋值操作，一行中简短声明多个变量时，":=" 语句中必须至少要声明一个新的变量。



## 基本数据类型

### 获取元素类型

```go
// 方法一，创建 reflect 变量
import "reflect"

var a int
typeOfA := reflect.TypeOf(a)
fmt.Println(typeOfA)
// 或者直接 fmt.Println(reflect.TypeOf(a))

// 方法二，使用 fmt 查看
import "fmt"
var a int
fmt.Printf("%T", a)   // 注意不能用 Println
```



### 整型

数字表示类型的比特长度，不同长度的类型是独立的，**混合计算会报错**

int 的长度**由硬件决定**：

| 数据类型 | 计算机架构 | 比特宽度 | 字节宽度 |
| -------- | ---------- | -------- | -------- |
| int      | 32位       | 32       | 4        |
| int      | 64位       | 64       | 8        |
| uint     | 32位       | 32       | 4        |
| uint     | 64位       | 64       | 8        |

也**可以指定 int 的固定长度类型**：

|                  数据类型                   | 比特宽度 |
| :-----------------------------------------: | :------: |
|                    int8                     |    8     |
|                    int16                    |    16    |
| int32 (rune, 可以用来表示一个 Unicode 字符) |    32    |
|                    int64                    |    64    |
|               uint8 （byte）                |    8     |
|                   uint16                    |    16    |
|                   uint32                    |    32    |
|                   uint64                    |    64    |



### 浮点类型

Golang 支持两种浮点型数值：float32 和 float64。可以使用 math 包中的常量获取浮点类型的边界值

```go
const (
    MaxFloat32             = 3.40282346638528859811704183484516925440e+38  // 2**127 * (2**24 - 1) / 2**23
    SmallestNonzeroFloat32 = 1.401298464324817070923729583289916131280e-45 // 1 / 2**(127 - 1 + 23)

    MaxFloat64             = 1.797693134862315708145274237317043567981e+308 // 2**1023 * (2**53 - 1) / 2**52
    SmallestNonzeroFloat64 = 4.940656458412465441765687928682213723651e-324 // 1 / 2**(1023 - 1 + 52)
)
```

注意：浮点类型有精度限制，计算结果不准确，不推荐使用
**需要进行浮点计算时，可以使用放大 100 倍的整数计算，计算完成再将结果缩小 100 倍**



### 复数

Golang 自带了复数类型，分别为 complex64, complex128，定义时需要制定实部和虚部，如：

```go
var c1, c2 complex64
c1 = 1 + 2i
c2 = 2 + 3i
fmt.Println(c1 + c2) // out: 3 + 5i
```



### 布尔

bool 类型，有 true 和 false 两种类型，要注意的是，和 python 不同，Golang 不会自动对变量识别 true or false，因此 if 1 这种方式是会报错的



### 错误

error，error 实际上是一种接口类型



### 字符

rune, byte 可用于表示单个字符，字符串，rune 数组，byte 数组之间可以相互转换，让我们从源码看看 byte 和 rune 的区别：

```go
// byte is an alias for uint8 and is equivalent to uint8 in all ways. It is
// used, by convention, to distinguish byte values from 8-bit unsigned
// integer values.
type byte = uint8


// rune is an alias for int32 and is equivalent to int32 in all ways. It is
// used, by convention, to distinguish character values from integer values.
type rune = int32
```

可以看出，byte 为 1 字节的 uint8 数值，rune 为 4 字节的 int32 数值

通过下面的代码，可以看出**一个中文字符占了三个字节**，在 []byte{} 中长度为 3，string 和 []byte{} 的长度相等，string 实际上就是由 byte 数组实现的。

```go
a := "测试T"
aRune := []rune(a)
aByte := []byte(a)
fmt.Println(a, aRune, aByte)
fmt.Println(len(a), len(aRune), len(aByte))
  
// output：
// 测试T [27979 35797 84] [230 181 139 232 175 149 84]
// 7 3 7
```



### 字符串

string 类型，go 的字符串只能用 “” 或者 原始字符串引号 ``(重音符号)

```go
// 普通字符串
s1 := "hello\nworld"    // 里面的 \n 会被转义为回车

// 原始字符串
s2 := `hello\nworld`    // 所有内容都是字符，不会被转义
```

字符串是不可修改的，对字符串进行修改操作时，会创建一个新的字符串赋值给当前字符串

```go
s := "hello"
s += " world" // 这样不会报错，会把 "hello world" 整个重新分配给 s

s[0] = "a"    // 这样写就会报错
```

因此，如果涉及到大批量的字符串拼接或者修改的操作，效率会很低且耗费资源，推荐使用 bytes 包的 Buffer 来进行操作

```go
import "bytes"

bs := bytes.NewBufferString("")
bs.WriteString("hello ")
bs.WriteString("world")
s := bs.String()  // out: hello world
```



### 字符串常用操作

#### 获取字符串长度

字符串本质上是字符数组，所以可以使用 len(s) 获取长度。
要注意的是对用户名等可能包含中文或其他字符的字符串进行判断时，使用 utf8 长度判断而不是 len() 判断，否则识别出来的长度是不正确的

```go
fmt.Println(len("测试"))								 // 6
fmt.Println(utf8.RuneCountInString("测试"))			 // 2
fmt.Println(utf8.RuneCountInString("混合测试,fight!"))  // 11
```

#### 数值和字符串的相互转换

##### int 转换为 string

int 转换为 string 不能直接 string(i)，这样转完会变成空值

```go
s := strconv.Itoa(i)
```

##### int64 转换为 string

```go
var i64 int64
s := strconv.FormatInt(i64,10)  // 第二个参数为基数（2~36），如十进制二进制等
```

##### string 转换为 int 型

使用非数值的字符串转换为数值时会发生错误，比如 “abc” 转为数字或者使用空字符串，因此需要接收 error。

```go
i, err := strconv.Atoi(s)
if err != nil {
	return err
}  
```

##### string 转换为 int64

```go
//第二个参数为基数（2~36），
//第三个参数位大小表示期望转换的结果类型，其值可以为0, 8, 16, 32和64，
//分别对应 int, int8, int16, int32和int64
i64, err := strconv.ParseInt(s, 10, 64)
```

##### string 和 slice 相互转换

**slice 转换为 string**

```go
import "strings"

sli := []string{"111","2222","333","4444"}
ss := strings.Join(s,"--")  // 输出为 111--2222--333--4444
```

**string 转换为 slice**

```go
ss := "a,b,c"
sli := strings.Split("a,b,c", ",")
```

**字符串替换**
使用 strings 包的 Replace 函数进行字符串替换

```go
// Replace
// 如果最后一个参数为正整数，表示将前 n 个 old 替换为 new
// 如果最后一个参数为负数，一般为-1，表示全局替换，相当于 ReplaceAll
func Replace(s, old, new string, n int) string
// 例：
fmt.Println(strings.Replace("oink oink oink", "oink", "moo", -1))
// output: moo moo moo
fmt.Println(strings.Replace("oink oink oink", "oink", "moo", 2))
// output: moo moo oink

// ReplaceAll
func ReplaceAll(s, old, new string) string
```



### 指针

如果用“var x int”声明语句声明一个x变量，那么 &x 表达式（取x变量的内存地址）将产生一个指向该整数变量的指针，指针对应的数据类型是 *int ，指针被称之为“指向int类型的指针”。同时 *p 表达式对应p指针指向的变量的值。

```go
x := 1
p := &x     // p 为指向 x 的指针   
/* 等价于：
var p *int
p = &x
*/
fmt.Println(*p)   // 输出 1
*p = 2     // 修改 *p 指向对象的值
fmt.Println(x)   // 输出 2
```

任何类型的指针的零值都是 nil。如果 p != nil 测试为真，那么 p 是指向某个有效变量。指针之间也是可以进行相等测试的，只有当它们指向同一个变量或全部是nil时才相等。



### 自定义类型

`type 类型名字 底层类型`

这种操作的意义在于赋予相同基本类型（比如 float64）的变量不同的含义，比如华氏温度和摄氏温度，这两种温度被定义成不同的数据类型，可以避免混用不同的温度单位，产生错误的计算结果。

```go
// Package tempconv performs Celsius and Fahrenheit temperature computations.
package tempconv
import "fmt"

type Celsius float64 // 摄氏温度
type Fahrenheit float64 // 华氏温度

const (
    AbsoluteZeroC Celsius = -273.15 // 绝对零度
    FreezingC Celsius = 0 // 结冰点温度
    BoilingC Celsius = 100 // 沸水温度
)

func CToF(c Celsius) Fahrenheit { 
    return Fahrenheit(c*9/5 + 32) 
}

func FToC(f Fahrenheit) Celsius { 
    return Celsius((f - 32) * 5 / 9) 
}
```



### 常量

go 使用 const 创建常量，它们在编译时被创建，只能是数字、字符串或布尔值。

```go
// 常量用的是 = 而不是 :=
const x = 42
```

可以使用 iota 生成枚举值，第一个iota 表示为0，当iota 再次在新的一行使用时，它的值累加 1。第一行之外的 = iota 可以省略，只要出现了一次 iota， 当前 const 结构中后面每一行 iota的值加1。

```go
package main

import "fmt"

func main() {
    const (
        a = iota  // 0
        b		 // 1
        c		 // 2
        d = "ha"  // "ha", iota += 1, iota = 3
        e		 // "ha", iota += 1, iota = 4
        f = 100   // 100, iota += 1, iota = 5
        g		 // 100, iota += 1, iota = 6
        h = iota  // 7
        i		 // 8
    )
    
    fmt.Println(a,b,c,d,e,f,g,h,i)
    // 结果为 0 1 2 ha ha 100 100 7 8
    
    // 另一个有趣的示例
    const (
        x = 1 << iota  // 1 << 0
        y = 3 << iota  // 3 << 1
        z			  // 3 << 2
        q			  // 3 << 3
    )
    
    fmt.Println(x,y,z,q)
    // 结果为 1 6 12 24
}
```



### 强制类型转换

```go
type_name(expression)
// 如 
f := float32(3)
i := int64(2)
```

注意：将长类型强制转换为短类型时会发生截断