# Java表达式的转型规则

Java表达式转型规则**由低到高**转换：

1、所有的byte,short,char型的值将被提升为int型；

2、如果有一个操作数是long型，计算结果是long型；

3、如果有一个操作数是float型，计算结果是float型；

4、如果有一个操作数是double型，计算结果是double型；

5、被fianl修饰的变量不会自动改变类型，当2个final修饰相操作时，结果会根据左边变量的类型而转化。



相关题目：

下列代码片段中，存在编译错误的语句是()

```java
byte b1=1,b2=2,b3,b6,b8;
final byte b4=4,b5=6,b7;
b3=(b1+b2);  /*语句1*/
b6=b4+b5;    /*语句2*/
b8=(b1+b4);  /*语句3*/
b7=(b2+b5);  /*语句4*/
System.out.println(b3+b6);
```



解析：

语句1错误：b3 = (b1+b2); 自动转为int，所以正确写法为 b3 = (byte)(b1+b2); 或者将b3定义为int；

语句2正确：b6 = b4+b5; b4、b5为**final类型，不会自动提升，所以和的类型视左边变量类型而定**，即b6可以是任意数值类型；

语句3错误：b8 = (b1+b4); 虽然b4不会自动提升，但b1仍会自动提升，所以结果需要强转，b8=(byte)(b1+b4);

语句4错误：b7 = (b2+b5); 同上。同时注意b7是final修饰，即只可赋值一次，便不可再改变。