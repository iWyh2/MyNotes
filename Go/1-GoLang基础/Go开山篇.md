# Go



# Go简介

> 由Unix和C语言创始人之一，肯汤普森发明
>
> 为的就是优化C语言的基础之上发明的GO
>
> Google赞助
>
> 为的是合理利用多核多CPU、简洁高效、还能处理大并发



Go语言吉祥物：gordon ![go语言吉祥物 的图像结果](https://tse4-mm.cn.bing.net/th/id/OIP-C.7lz8QNYHKfo96_xmpIIq9AAAAA?w=119&h=180&c=7&r=0&o=5&pid=1.7)



Go语言特点：

* 语法简洁
* 开发效率高
* 执行性能好
* 天生支持并发



Go语言的包的概念：Go的每一个文件都属于一个包，不能单独存在

Go语言有垃圾回收机制，内存自动回收

GO语言的函数可以返回多个值

Go语言语句后面不需要带分号，编译器自动给你带上分号



## [知识补充] Go开发环境搭建(Windows系统)

1. 安装配置SDK（Software Development Kit：软件开发工具包）
2. 配置系统环境变量
   * GOROOT：GO的安装目录
   * GOPATH：工作目录，也就是写的代码放在哪儿
   * Path：里面配置Go安装目录中的bin的目录



Go语言开发的目录结构：

* 开发目录：GoDocument
  * bin
  * pkg
  * src（放创建的Go项目）
    * project01
      * main
        * 源码：hello.go
      * package
    * project02
    * .....



Go语言的快速开发：

* 创建一个hello.go
* 然后cmd，go build hello.go 生成了exe文件
* 然后hello.exe运行文件