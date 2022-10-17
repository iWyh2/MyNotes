# final关键字的作用



1、final关键字的含义

final表面意思就是不可更改的,恒量的意思；**类似于C语言中的const关键字，指的是无法改变的量**，这与静态标量static是有区别的，**静态变量指的是只有一份存储空间，值是可以改变的**。

在Java中**final修饰的就是常量**，而且变量名要大写；

Math类：public static final double E = 2.7182818284590452354;public static final double PI = 3.14159265358979323846; Java源码中好多变量都用final修饰



2、final的作用

final根据修饰位置的不同作用也不相同，针对三种情况：

（1）修饰变量，被final修饰的**变量必须要初始化，赋初值后不能再重新赋值**。

注意：局部变量不在我们讨论的范畴，因为局部变量本身就有作用范围，不使用private、public等词修饰。

（2）修饰方法，被final修饰的**方法不能重写**。

（3）修饰类，被final修饰的**类不能够被继承**。

注意：final修饰的类，类中的所有成员方法都被隐式地指定为final方法。

2.1、final修饰变量

被final修饰的变量必须显示的初始化，初始化可以以三种方式：1）**定义时**初始化，2）在**构造器中**设置值，3）在**非静态块中**为final实例变量设置值。

final修饰变量指的是：这个变量被初始化后便不可改变，这里不可改变的意思**对基本类型来说是其值不可变**，而**对于对象变量来说其引用不可变，即不能再指向其他的对象**。

```java
public class Test01{    
    final int x1= 10000;    
    final int x2;    
    final int x3;  
    {    
        x2 = 20000;    
    }  
    Public exe3(){        
        this.x3 = 3000;    
    }}
```

如果final修饰的变量是**对象类型，那么不可更改指的是该变量不可以再指向别的对象，但是对象的值时可以更改的**，比如：

```java
final Operate operate = new Operate() ;// operate有一个普通变量i 初始化operate.i = 11;
operate.i = 12;//更改为12
System.out.println(operate.i); //输出12上述是自定义类，即便是数组，List等集合类型，所保存的值也是可以更改的。
```



3、**final和static的区别**

**static作用于成员变量用来表示只保存一份副本**，而**final的作用是用来保证变量不可变**，看一下网上的一个例子：

```java
public class Test {  
    public static void main(String[] args) {    
        MyClass myClass1 = new MyClass();    
        MyClass myClass2 = new MyClass();    
        System.out.println(myClass1.i);    
        System.out.println(myClass1.j);    
        System.out.println(myClass2.i);    
        System.out.println(myClass2.j);   
    }}
class MyClass {  
    public final double i = Math.random();  
    public static double j = Math.random();}//运行结果，两次打印，两个对象的j的值都是一样的，j是static类型的属于类，在类加载时就已经确定了值，因此两次值相同。i不是static的因此属于对象，在实例化时才会初始化好值，所以两次打印值不同，但是i的值是不可变的，j的值可变
```



4、其他final相关的知识

（1）使用final关键字，如果编译器能够在编译阶段确定某变量的值，那么编译器就会**把该变量当做编译期常量来使用**。如果需要在运行时确定，那么编译器就不会优化相关代码。

```java
public class Test {    
    public static void main(String[] args) {    
        String a = "hello2";     
        final String b = "hello";    
        String d = "hello";    
        String c = b + 2;     
        String e = d + 2;    
        System.out.println((a == c));//true  
        System.out.println((a == e));//false  
    }}  
//final类型的b，在编译阶段能够确定值，不是变量了，是常量。且这个常量值（“hello”）在字符串常量池，加上2后由于Java优化机制，使其在字符串常量池寻找“hello2”，找到并赋予c，所以与a这个字符串常量池的“hello2”地址相同，所以相等为true
//非final类型的d，在编译阶段确定不了，为变量，变量字符串加上2，在堆区创建新的变量，与a这个存在于字符串常量池的“hello2”地址不一样，所以不相等为false

//详情请见 [Java的String类问题.md]

public class Test {        
    public static void main(String[] args) {      
        String a = "hello2";       
        final String b = getHello();      
        String c = b + 2;       
        System.out.println((a == c));//false   
    }     
    public static String getHello() {        
        return "hello";  
    }}
//即便是final类型，编译阶段也确定不了值。因为这个初始化为调用的静态方法，返回一个字符串字面量，静态方法在栈内存中，返回的字符串也在栈内存，编译阶段确定不了值，所以认为b为一个变量，所以c也为创建的变量，所以与字符串常量池（在堆内存）的a不相等，输出false
```

（2）注意不要将final与finally、finalize()等搞混。

（3）将类、方法、变量声明为final能够提高性能，这样JVM就有机会进行估计，然后优化。

（4）接口中的变量都是public static final 的



> ©iWyh2