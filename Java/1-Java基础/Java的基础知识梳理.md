# Java的基础知识梳理

> JavaSE的内容梳理 📖

## 1. Java的特性

* 面向对象
* 健壮性（强类型，异常处理，垃圾的自动收集）
* 跨平台性
* 解释型语言（编译后的代码不能直接被机器执行）





## 2. Java的流程控制

在进行程序设计的时候，我们会经常进行逻辑判断，根据不同的结果做不同的事，或者重复做某件事，我们对类似这样的工作称为流程控制。在Java中，流程控制分为两大类：**选择**和**循环**。





## 3. Java的数组与数组工具类

C/C++风格

```c++
声明数组变量：dataType arrayRefVar[]; 
```

Java也有这样的写法，但不推荐。

* Java风格：

```java
声明数组变量：dataType[] arrayRefVar;
创建数组：arrayRefVar = new dataType[arraySize]
```

* Java数组的应用场景：
  * **业务数据个数固定，且都是同一批数据类型**

Java的**数组工具类：Arrays**

* 数组操作工具类，专门用于操作数组元素的
* 常用API：
  * （static）toString(类型[] arr)：返回数组内容字符串
  * （static）sort(类型[] arr)：对数组进行默认升序排序
  * **（static）sort(类型[] arr, Comparator<？super T> com)**：使用比较器对象进行自定义排序，可以匿名内部类，重写compare方法
    * 如果认为左边数据 大于 右边数据，返回正整数
    * 如果认为左边数据 小于 右边数据，返回负整数
    * 如果认为左边数据 等于 右边数据，返回0
  * （static）binarySearch(int[] arr，int key)：二分法搜索数组中的数据，存在便返回索引，不存在返回-1
  * **（static）asList(放置多个相同类型元素)**：将生成的数组转为Collection里的ArrayList





## 4. Java的自动类型转换

* 精度小的给精度大的

* byte -> short -> int -> long -> float -> double

​              				char↑





## 5. Java的表达式类型提升

* 表达式中，小范围型的变量会自动转换成当前较大范围的类型再运算

* byte short char -> int -> long -> float -> double
* 表达式的最终结果类型由表达式中的最高类型决定
* 表达式中，byte short char 直接转换成int进行运算





## 6. Java的强制类型转换

* 强行将类型范围大的赋值给类型范围小的
* 会造成数据丢失或溢出
* 浮点型强转整型会丢失小数部分，只保留整数部分





## 7. Java的static关键字

* 静态方法只能访问静态的成员
* 实例方法可以访问静态的也可以访问实例的成员
* 静态方法中不可以有this
* 饿汉单例设计模式（已经造好了）
* 懒汉单例设计模式（需要时再创建）





## 8. Java的代码块

* 是Java类的五大成分之一
* 定义在类中，方法之外
* 静态代码块（static{}）：随着类的加载而加载，自动触发，只执行一次，用于初始化静态数据便于后续使用
* 构造代码块（{}）：在每次创建对象实例化类时，在调用构造器之前先调用这个代码块，用于初始化实例资源





## 9. Java的final关键字

* 修饰类：为最终类，不能被继承
* 修饰方法：为最终方法，不能被重写
* 修饰变量：为常量，不能被修改
  * 修饰基本类型变量：变量存储的数据值不能改变
  * 修饰引用类型变量：变量存储的地址值（对象）不能改变，但地址指向的对象内容可以改变





## 10. Java的常量

* 由**public static final**修饰的成员变量
* 必须要有初始化值
* 执行时其值无法改变
* 执行原理：编译阶段进行”宏替换“，常量替换为真实字面量
* 命名规范：全部大写
* 用于做系统的配置信息，提高可读性





## 11. Java的枚举

* 是Java的特殊类

* ```java
  格式：
  修饰符 enum 枚举名称 {
  	第一行罗列枚举类实例名称;
  }
  ```

* 最终类，不能被继承

* 不能创建对象

* 相当于多例模式

* 用于信息标志与分类





## 12. Java的重载

* 一个类中，多个方法名相同，但形参列表不同，则为重载方法
* 重载 方法名必须相同，参数类型必须不同，包括但不限于一项，参数数目，参数类型，参数顺序
  当你的方法名与参数类型与父类相同时，已经是重写了，这时候如果返回类型或者异常类型比父类大，或者访问权限比父类小都会编译错误





## 13. Java的封装

* 用于正确设计对象的属性和方法
* 原则：对象代表什么，就要封装对应的数据，并提供对应的行为





## 14. Java的继承

* ```java
  格式：
  public class 子类名 extends 父类名 {}
  ```

* 子类继承了父类，可以直接使用父类的公共属性与方法。能继承父类的私有成员，但无法直接使用。不会继承父类的静态成员，这是共享。

* 不会继承父类的构造器

* 单继承，只继承一个父类

* Java不支持多继承，支持多层继承

* 所有类都是Object的子类（默认的）

* 在子类方法中访问成员，为就近原则，从自己开始往上级寻找

* super指向父类，this指向本类

* **子类有与父类一样的方法声明则就是重写了父类的该方法**，可加@Override校验

* 父类的私有方法与静态方法不能被重写

* 子类继承父类后，创建对象时，**先默认访问父类的的无参构造器，再访问自己的构造器**，因为要先完成父类的数据空间的初始化，所以**子类的构造器默认第一行不写也存在super()**

* 可以调用父类的有参构造器，用于初始化继承自父类的数据

* **父类必须要有无参构造器**，不然子类构造器默认访问父类无参构造器会报错，除非手动调用父类有参构造器

* this可以调用本类其它构造器，在构造器中this(..)不能与super(..)一起存在

* **重写** 要求**两同两小一大原则**， **方法名相同，参数类型相同**，**子类返回类型小于等于父类方法返回类型**， **子类抛出异常小于等于父类方法抛出异常**， **子类访问权限大于等于父类方法访问权限**。［注意：这里的**返回类型必须要在有继承关系的前提下比较**］

  当你的方法名与参数类型与父类相同时，已经是重写了，这时候如果返回类型或者异常类型比父类大，或者访问权限比父类小都会编译错误

  也就是**重写标准**：

  1. 参数列表必须与被重写方法的相同。（方法名相同，参数类型相同
  2. 重写方法不能限制比被重写方法更严格的访问级别（子类访问权限大于等于父类方法访问权限
  3. 重写方法不能抛出新的异常或者比被重写方法声明的检查异常更广的检查异常（子类抛出异常小于等于父类方法抛出异常
  4. 返回类型必须与被重写方法的返回类型相同（**基础类型时**）。**仅当返回值为类类型时**，重写的方法才可以修改返回值类型，且必须是父类方法返回值的子类。（返回类型必须要在有继承关系的前提下比较，子类返回类型小于等于父类方法返回类型

* **子类从其父类继承所有成员（字段，方法和嵌套类）。 构造函数不是成员，所以它们不被子类继承，但是可以从子类调用超类的构造函数**。





## 15. Java的抽象类

* ```java
  格式：
  修饰符 abstract class 类名{
  	修饰符 abstract 返回值类型 方法名称(形参列表);
  }
  ```

* 为抽象类 和 抽象方法

* 定义了抽象方法必须声明为抽象类

* 抽象方法只有签名没有方法体

* 抽象类也就是不完整的设计图，交由子类继承并完成

* 子类必须重写完抽象父类的全部抽象方法

* 不能创建对象

* final与abstract为互斥关系

* 模板方法模式：定义抽象类，再在类中定义通用且确定的部分代码为模板方法，其他不同的不确定的定义为抽象方法交由继承的子类自定义，抽象方法会被模板方法在方法体中某处被调用，给模板方法加上final，使其不会被子类重写，更安全优雅

  ```java
  public abstract class Student{
      public final void write() {
          ...//已经写好的逻辑内容
          this.writeMain();
          ...//已经写好的逻辑内容
      }
      
      public abstract String writeMain();//需要我们自己自定义重写的方法
  }
  ```
  
* 关于抽象类

  JDK 1.8以前，抽象类的方法默认访问权限为protected

  JDK 1.8时，抽象类的方法默认访问权限变为default

  





## 16. Java的接口

* JDK-8之前的老接口格式：

  ```java
  public interface 接口名{
  	//常量
      //抽象方法
  }
  ```

* 接口是一种规范，默认为public abstract/public static final

  Java8才开始可以被abstract修饰

* 子类（实现类）用implements实现接口

* 要重写完抽象方法

* 类与接口：多实现（一个类可实现多个接口）

* 接口与接口：多继承（一个接口可继承多个接口）

* JDK-8之后，允许在接口中直接定义带有方法体的方法

  * 默认方法：类似于写实例方法，但要加上**default**修饰（既可以是public，也可以是default）

    ```java
    default void xxx(){
    	...
    }
    ```

    实现类对象才能调用

  * 静态方法：static修饰（默认public）

    ```java
    static void xxx(){
    	...
    }
    ```

    **必须用接口本身调用**

  * 私有方法（JDK 1.9以上才有）

    ```java
    private void xxx(){
    	...
    }
    ```

    只能在本类中被其他默认方法或私有方法访问

* 接口不能创建对象

* 一个类实现多个接口，多个接口中有同样的静态方法不冲突

* 一个类继承了父类，同时又实现接口，父类和接口中有同名方法，默认用父类实现的方法

* 一个类实现了多个接口，多个接口中存在同名的默认方法，不冲突，这个类重写该方法即可

* 一个接口继承多个接口，没有问题，但多个接口中存在规范冲突则不能多继承

* 关于接口

  JDK 1.8以前，接口中的方法必须是public的

  JDK 1.8时，接口中的方法可以是public的，也可以是default的

  JDK 1.9时，接口中的方法可以是private的





## 17. Java的多态

* 同类型的对象执行同一个行为，会表现出不同的行为特征

* 常见形式：

  ```java
  父类类型 对象名称 = new 子类构造器;
  接口 对象名称 = new 接口实现类构造器;
  ```

* 成员访问特点：

  * 方法调用：**编译**看左边（父），**运行**看右边（子）
  * 变量调用：编译看左边，运行也看左边（**多态侧重于行为上的多态**）

* 使用多态的前提：有继承与实现的的关系，有父类引用指向子类对象，有方法重写

* 可实现右边对象的解耦合

* 父类做参数，方法可接收父类的一切子类对象（自动类型转换）

* **多态下不能使用子类独有的功能**，用**强制类型转换**解决即可

* **instanceof**关键字：`变量名 instanceof 真实类型`，判断关键字左边的变量指向的类型，是否是右边的类型或子类类型，返回boolean值，防止出现强制类型转换时的**ClassCastException**异常





## 18. Java的内部类

* 内部类就是**定义在一个类里面的类**

  ```java
  public class People{
      public class Heart{}
  }
  ```

* 类的五大成分之一

* 使用场景：

  * 一个事物的内部，还有一个部分需要一个完整的结构进行描述，且这个内部的完整结构只为外部这个事物提供服务，那么这个完整的结构可以为内部类
  * 内部类可以方便访问外部类成员（包括私有）

* 内部类可用private protected修饰

* 分类：

  * 静态内部类：有static修饰，属于外部类本身，特点与使用的普通类一致，只是在一个类里面

    ```java
    public class People{
        public static class Heart{}
    }
    ```

    * 实例化格式：

      ```java
      外部类名.内部类名 对象名 = new 外部类名.内部类名构造器;
      Outer.Inner in = new Outer.Inner();
      ```

    * 静态内部类中可以直接访问外部类的静态成员（静态只有一份，且被共享）

    * 静态内类中不可以直接访问外部类的实例成员（非静态成员），要实例化外部类访问

    * 编译之后的文件：**外部类$内部类.class**

  * 成员内部类（为非静态的成员变量）：无static修饰，JDK16开始可以再其内部定义静态成员

    * 实例化格式：

      ```java
      外部类名.内部类名 对象名 = new 外部类构造器.new 内部类名构造器;
      Outer.Inner in = new Outer().new Inner();
      ```

    * 成员内部类中可以直接访问外部类的静态成员

    * 也可以直接访问外部类的实例成员（因为要先实例化外部类才有内部类对象，所以可以直接访问）

    * 成员内部类中访问外部类对象，用**外部类名.this**

    * 编译之后的文件：**外部类$内部类.class**

  * 局部内部类：存在于方法，代码块，构造器中的，很鸡肋。无需了解。类文件名为外部类$N内部类.class

  * **匿名内部类**：**本质上是一个没有名字的局部内部类**，定义在方法、代码块、构造器等中，**只在这些区域内有作用**

    * 作用：**方便创建子类对象，为了简化代码编写，省去再创建新的子类源文件**

    * 格式：

      ```java
      new 类名/抽象类名/接口名(){
          //重写方法
      };
      ...
      Employee employee = new Employee() {
        public void work() {
            ...
        }  
      };
      ```

    * 是一个没有类名的内部类

    * 匿名内部类写出来就会产生一个匿名内部类的对象（写出类的同时有了对象）

    * **创建的匿名内部类的对象类型是当前new的类型的子类**（写匿名内部类就是写多态语法）

    * 使用形式：

      * **作为方法的实际参数进行传输**，可省去创建父类引用，直接new传参
  
* 编译之后的文件：**外部类$数字.class**





## 19. Java的Lambda表达式

* JDK-8开始的新语法格式

* 作用：简化匿名内部类的代码写法

* 简化格式：

  ```java
  (匿名内部类被重写方法的形参列表) -> {
      //被重写的方法的方法体
  }
  ```

* Lambda**只能简化函数式接口**的匿名内部类

* **函数式接口：接口，有且仅有一个抽象方法**。可在该接口上加@FunctionalInterface注解，表示这必须是个函数式接口

* ->：是语法，无实际意义

* 如：

  ```java
  @FunctionalInterface
  interface Swimming{
      void swim();
  }
  void goSwim(Swimming swimming){}
  ...
  public static void main(String[] args) {
      goSwim(new Swimming(){
          @Override
          void swim() {
              System.out.print("swim so fast");
          }
      });
      //-------下面为相同效果的Lambda简化--------
      goSwim(() -> {
              System.out.print("swim so fast");
          });
  }
  ```

* 省略写法（在简化的基础之上继续简化）

  * 参数类型可以省略不写

  * **只有一个参数**，那么可以**省略参数类型与小括号()**

  * 被重写方法的**方法体只有一行代码，省略大括号和分号**

  * 被重写方法的**方法体只有一行return语句，省略大括号、return、分号**

  * 就算没有参数也还是要写上小括号()

  * 如：

    ```java
    @FunctionalInterface
    interface Swimming{
        String swim(String);
    }
    void goSwim(Swimming swimming){}
    ...
    public static void main(String[] args) {
        goSwim(new Swimming(){
            @Override
            String swim(String s) {
                System.out,print("swim so fast");
            }
        });
        //-------下面为相同效果的Lambda简化--------
        goSwim(s -> {
                System.out.print("swim so fast");
            });
        //-------继续简化--------
        goSwim(s->System.out.print("swim so fast"));
        
        //----------------------------------
        goSwim(new Swimming(){
            @Override
            String swim(String) {
                return "我不会游泳";
            }
        });
        //------等于---------
        goSwim(s->"我不会游泳");
    }
    ---------------------------------------
    Arrays.sort(args, new Comparator<Integer>(){
        @Override
        public int compare(Integer o1,Integer o2) {
            return o1 - o2;
        }
    });
               //等于
    Arrays.sort(args,(o1,o2)->o1 - o2);
    ```

* 当用于遍历集合时，forEach方法用Lambda格式简化，如果遍历集合的目的是为了打印内容，最终Lambda会简化为

  ```java
  list.forEach(s->System.out.println(s));
  ```

  这里有一个Lambda的新格式：**方法引用**，满足以上条件，只有一个参数，一行代码，且为引用别人的方法，引用别人的方法也是传入刚刚那个参数时

  ```java
  list.forEach(s -> System.out.println(s));
  //方法的使用参数和引用方法传入的参数一致时  方法引用简化
  list.forEach(System.out::println);
  //或者引用的是构造器方法
  list.stream().map(s -> new Student(s)).forEach(s -> System.out.println(s));
  list.stream().map(Student::new).forEach(System.out::println);
  ```





## 20. Java的各类常用API

* Object类：
  * toString()：默认返回对象在堆内存里的地址信息
    * 重写，以便看到对象内容
  * equals()：默认比较当前对象与另一个对象的地址是否相同
    * 完全可以直接用“==”比较两个引用地址是否相同（也就是比较对象地址是否相同）
    * 子类重写Object的equals，自定义equals的比较规则（如比较两个对象内容是否相同）



* Objects类：
  * （static）equals(o1,o2)：**进行了非null检验**，更安全（阿里巴巴开发手册曾有，若用Object的equals，调用者必须为非null），会调用对象重写的equals方法
  * （static）isNull(o)：判断变量是否为空（就是return o==null）



* StringBuilder类（可变字符串类，是一种手段工具，最终还是要变回String类型）：
  * 构造器方法：StringBuilder() 创建空白可变的字符串对象/StringBuilder(String str) 创建指定内容的可变字符串对象
  * **append(任意类型)**：添加数据并返回StringBuilder对象本身 -> 可以链式编程
  * reverse()：将对象的内容反转
  * length()：返回对象内容长度
  * toString()：把StringBuilder转换为String



* **String类**：
  * **equals()：比较字符串内容是否一致**
  * **equalsIgnoreCase()：忽略大小写比较字符串内容是否一致**
  * **length()：返回字符串长度**
  * **charAt(int index)：返回索引处的字符**
  * **toCharArray()：转换为字符数组返回**
  * **substring(int beginIndex，int endIndex)：根据起始与结束索引截取新字符串，包前不包后，不包括结束索引位置的字符**
  * **substring(int beginIndex)：从开始索引到结束截取新字符串**
  * **replace(CharSequence target，CharSequence replacement)：返回替换目标值后的新字符串（CharSequence就是一个字符串）**
  * **split(String regex)：根据传入的规则切割字符串，返回字符串数组（regex就是源字符串内的需要分割的间隔符，比如逗号）**
  * **contains(CharSequence s)：判读是否包含传入的字符串**
  * **startWith(String prefix)：判断字符串是否以传入的字符串开始**



* Scanner类：
  * nextInt()：接收用户键盘输入的数字
  * next()：接收用户键盘输入的字符串



* Random类：
  * nextInt(n)：生成0 ~ n-1之间的随机数



* Math类（工具类）：
  * abs(int a)：返回参数a的绝对值
  
  * ceil(double a)：返回a向上取整的值
  
    ceil 方法上有这么一段注释：If the argument value is less than zero but greater than -1.0, then the result is negative zero
  
    如果参数小于0且大于-1.0，结果为 -0
  
  * floor(double a)：返回a向下取整的值
  
    ceil 和 floor 方法 上都有一句话：
  
    If the argument is NaN or an infinity or positive zero or negative zero, then the result is the same as the argument，意思为：如果参数是 NaN、无穷、正 0、负 0，那么结果与参数相同（如果是 -0.0，那么其结果是 -0.0）
  
  * round(float a)：返回a四舍五入后的值
  
  * max(int a, int b)：返回两个参数中的最大值
  
  * pow(dounle a, double b)：返回a的b次幂的值
  
  * random()：返回0.0 ~ 1.0（不含1.0）的double随机值



* System类（工具类）：
  * exit(int status)：终止当前Java虚拟机，传入参数非零表示异常终止
  * **currentTimeMillis()：返回当前系统时间的毫秒值形式**
  * arraycopy(src数组，起始索引，dest数组，起始索引，拷贝个数)：拷贝数组



* BigDecimal类：用于**解决浮点型运算精度失真问题**
  * （static）valueOf(double val)：将传入的浮点数参数包装为BigDecimal对象（使用之后API的第一步，获取BigDecimal对象）（阿里巴巴开发建议使用）
  * BigDecimal(String val)：阿里巴巴额外建议，若用构造方法创建BigDecimal对象，用String做参数的，直接传值会丢失精度
  * add(BigDecimal b)：加法
  * subtract(BigDecimal b)：减法
  * multiply(BigDecimal b)：乘法
  * divide(BigDecimal b)：除法
  * divide(BigDecimal b，精确几位，舍入模式)：除法（结果为无限、循环小数时）（舍入模式：RoundingMode.HALF_UP 四舍五入）



* **Date类**：当前系统此刻的日期时间（1s = 1000ms）
  * Date()：创建当前的日期对象
  * getTime()：获取当前日期对象的毫秒值
  * Date(long time)：将传入的时间毫秒值参数转换为日期对象
  * setTime(long time)：为日期对象将传入时间毫秒值，并设置为当前时间



* SimpleDateFormat类：**设置Date日期与时间毫秒值的格式**/**将字符串时间解析为日期对象**
  * SimpleDateFormat()：创建SimpleDateFormat对象，使用默认格式
  * SimpleDateFormat(String pattern)：创建SimpleDateFormat对象，使用pattern指定格式
    * pattern：
      * `yyyy年MM月dd日 HH:mm:ss EEE a`:  EEE-星期几/a-上下午
  * **format(Date date)：将日期对象转为时间字符串返回**
  * **format(Object time)：将时间毫秒值转为时间字符串返回**
  * **parse(String source)：将时间字符串解析为日期对象返回，格式要一致才能解析**



* Calender类（抽象类）：代表此刻系统日期对应的日历对象
  * getInstance()：获取Calender对象
  * get(int filed)：返回日历对象的某个字段信息（**不知道字段，可以先打印一下Calender对象，看有哪些字段**）
  * set(int field，int value)：修改日历中的某个字段信息（一般不会修改日历时间，修改之后就不是此刻的日历对象）
  * add(int field，int amount)：为某个字段增加或减少指定的值（单位为天、分等）
  * getTime()：返回此刻的日期对象
  * getTimeInMillis()：返回此刻的时间毫秒值



* Java-8新增日期类API：严格区分 时刻、本地日期、本地时间，方便运算，且都为不变类型，不必担心被修改
  * LocalDate（日期）/LocalTime（时间）/LocalDateTime（日期时间）：API通用
    * （static）now()：根据当前时间创建对象（LocalXxxx.now()）
    * （static）of(...)：指定日期/时间创建对象（LocalDate.of(2002,9,20)/LocalTime(11,11,11)）
    * getXxx()：获取时间对象的Xxx字段信息
    * LocalDateTime可以转为另外两种对象：
      * toLocalDate()：转为一个LocalDate返回
      * toLocalTime()：转为一个LocalTime返回
    * 运算方法，并返回一个新的日期时间对象：
      * plusXxx/minusXxx/withXxx/isXxx

  

  * Instant类（时间戳）：包含日期与时间，类似于Date，两者可以互相转换，功能更丰富
    * （static）now()：获取当前时间戳（世界时间）
    * atZone(ZonId.systemDefault())：使用系统默认时区
    * Date.form(Instant time)：将时间戳转为Date返回
    * date.toInstant()：将Date转为时间戳返回

  

  * DateTimeFormatter类（线程安全相关的日期时间格式器）
    * 正反都能调用format方法（不管是想格式化的时间对象还是DateTimeFormatter对象）
    * （static）ofPattern(String pattern)：指定格式获取DateTimeFormatter对象
    * 也可以解析字符串时间：parse

  

  * Period类（日期间隔）：计算日期间隔，用于LocalDate
    * （static）between(LocalDate d1，LocalDate d2)：获取时间间隔对象
    * getYears()/getMonths()/getDays()：计算相隔多少年、月、日

  

  * Duration类（时间间隔）：计算时间间隔，用于LocalDateTime或Instant
    * （static）between(时间参数1，时间参数2)：获取时间间隔对象
    * toDays/toHours/toMinutes/toMillis/toNanos：计算相隔多少时间

  

  * ChronoUnit类（工具类）：可用于所有时间单位，测量一段时间
    * 如：ChronoUnit.YEARS.between(LocalDateTime1，LocalDateTime2)

(更多请查询API文档)





## 21. Java的包装类

* 就是八种基本类型对应的的引用类型，一切皆对象的思想
* Byte、Short、Integer、Long、Character、Float、Double、Boolean
* 用于集合与泛型
* 自动“装箱”、”拆箱“：**基本类型数据和变量可以直接赋值给包装类型变量，反之也可以**
* 默认值为null
* 特有功能：
  * 把基本类型数据转为字符串格式：
    * 包装类对象.toString()/包装类名.toString(基本类型数据)
  * **把字符串类型数值，转换成真实的数据类型**
    * 包装类名.parseXxx(“字符串类型的相应数据”)
* 对于double类型数值还可以比较大小且不用强转为整导致比较出问题
  * Double.compare(double1,double2)





## 22. Java的正则表达式

* 正则表达式就是用规定的字符制定规则，用来**校验数据格式的合法性**

* 匹配规则：

  | 字符类                                    |                                                              |
  | ----------------------------------------- | ------------------------------------------------------------ |
  | `[abc]`                                   | 只能是a，b，或者c                                            |
  | `[^abc]`                                  | 除了abc之外的任何字符                                        |
  | `[a-zA-Z]`                                | `a` through `z` or `A` through `Z`, inclusive (range)        |
  | `[a-d[m-p]]`                              | `a` through `d`, or `m` through `p`: `[a-dm-p]` (union)      |
  | `[a-z&&[def]]`                            | `d`, `e`, or `f` (交集)                                      |
  | `[a-z&&[^bc]]`                            | `a` through `z`, except for `b` and `c`: `[ad-z]` (subtraction) |
  | `[a-z&&[^m-p]]`                           | `a` through `z`, and not `m` through `p`: `[a-lq-z]`(subtraction) |
  | **Predefined character classes**          |                                                              |
  | `.`                                       | 任何字符                                                     |
  | `\d`                                      | A digit: `[0-9]`                                             |
  | `\D`                                      | A non-digit: `[^0-9]`                                        |
  | `\s`                                      | A whitespace character: `[ \t\n\x0B\f\r]`                    |
  | `\S`                                      | A non-whitespace character: `[^\s]`                          |
  | `\v`                                      | A vertical whitespace character: `[\n\x0B\f\r\x85\u2028\u2029]` |
  | `\V`                                      | A non-vertical whitespace character: `[^\v]`                 |
  | `\w`                                      | A word character: `[a-zA-Z_0-9]` 字母，数字，下划线          |
  | `\W`                                      | A non-word character: `[^\w]` 除了字母，数字，下划线         |
  | **贪婪的量词**(X可以是上表所有规则表达式) |                                                              |
  | *X*`?`                                    | *X*, once or not at all                                      |
  | *X*`*`                                    | *X*, zero or more times                                      |
  | *X*`+`                                    | *X*, one or more times                                       |
  | *X*`{`*n*`}`                              | *X*, exactly *n* times（正好n次）                            |
  | *X*`{`*n*`,`}                             | *X*, at least *n* times                                      |
  | *X*`{`*n*`,`*m*`}`                        | *X*, at least *n* but not more than *m* times                |

* String类的**匹配正则表达式规则**的API：

  * **matches(String regex)**：判断字符串是否匹配正则表达式，匹配返回true，不匹配返回false

* 正则表达式在字符串内的使用：

  * replaceAll(String regex, String newStr)：按正则表达式匹配的内容进行替换
  * split(String regex)：按照正则表达式匹配的内容进行分割字符串，返回一个包含分割内容的字符串数组

* 爬取信息：

  * 首先有一堆掺杂邮箱电话号码的字符信息str

  * 定义一个爬取规则

    ```java
    String regex = "(\\w{1,}@\\w{2,10}(\\.\\w{2,10}){1,2})|"        //xxx@xxx.xxx
        		 + "(1[3-9]\\d{9})|" 								//1XXXXXXXXXX
        		 + "(0\\d{2,5}-?\\d{3,8}-?\\d{3,8})"      			//0xxxxx-xxxx-xxxxxx 可能没有(-)符号
    ```

  * 把正则表达式转换为一个匹配规则对象

    ```java
    Pattern pattern = Pattern.compile(regex);
    ```

  * 用匹配规则对象去扫描需要爬取的掺杂信息获取匹配器对象

    ```java
    Matcher matcher = pattern.matcher(str);
    ```

  * 通过匹配器对象去爬取所需格式信息

    ```java
    while(matcher.find()) {//直到爬取完为止
    	System.out.println(matcher.group());//爬取出一组匹配信息
    }
    ```

    



## 23. Java的集合

* 概述：大小不固定，可以动态变化，类型也不固定，适合元素个数不确定，且**要增删的场景**，还有许多API可以调用

* 和数组一样都是容器，**存储对象**，不支持基本类型，要用就**用包装类**

* 适用场景：**数据个数不确定，且要增删**

* 集合分为**Collection(单列集合)**和**Map(双列集合)**
  * **Collection**（单例集合祖宗接口）：体系接口，每个元素只包含一个值
  
    常用API：
  
    ```java
    1. add，在末尾添加
    2. clear，清空
    3. size，集合大小，元素个数
    4. contains，是否包含
    5. remove，不支持索引，直接删除指定值，且只删除一次，删除前面先出现的
    6. toArray，转为Object数组
    7. addAll，将调用者”倒入“参数对象
    8. isEmpty，判断是否为空
    ```
  
    遍历方式：
  
     1. **迭代器**：Iterator，**集合专用的遍历方式**
  
        获取方式：**集合对象.iterator()**，返回该集合的迭代器对象，一开始指向0位置的元素
  
        API：hasNext（当前位置是否有元素）/**next**（获取当前位置的元素，同时将迭代器移向下一个位置，要小心会越界）所以一般都是两个一起使用
  
     2. **增强for循环**（forEach）：**即可遍历集合，又可以遍历数组，内部原理为迭代器**，用来遍历集合也就是简化迭代器写法。实现了iterable接口的类才可以使用迭代器和增强for，Collection系列全部实现了。这个相当于拷贝了一份集合中的元素
  
        格式：for(元素数据类型 变量名：数组或Collection集合){}
  
     3. **Lambda遍历集合**：Iterable接口提供的方法-> **forEach方法**
  
        格式：forEach(Consumer<? super T> action)，需要重写内部accept方法，所以可以**结合Lambda简化省略并遍历做行为**，直接用**匿名内部类**
  
    
  
    * **List**（接口）：体系接口，这个系列添加的元素**都是有序、可重复、有索引的**
  
      List独有API：
  
      ```java
      add(int index，E element)：在指定位置插入指定元素
      
      remove(int index)：删除指定位置处元素，返回被删除的元素
      
      set(int index，E element)：修改指定位置处的元素，返回被修改的元素
      
      get(int index)：返回指定索引处的元素，快速查询，体现数组的特性，快速取元素
      ```
  
      List的遍历除了Collection的三种方式，还可以用最普通的for，因为有索引
  
      
  
      * **ArrayList**（基于**数组**数据结构）：初次创建，默认容量为10。满了后每次1.5倍扩容。增删为移动元素实现。查询块，增删慢。
  
        常用API：基本和List接口API一致
  
        独有的为：boolean remove(Object o)：删除指定元素，返回是否删除成功
  
      
  
      * **LinkedList**（基于**双向链表**数据结构）：首位操作极快，查询慢。LinkedList可以完成**栈**（头插）和**队列**（尾插），都是首尾操作。
  
        LinkedList特有API：
  
        ```java
        addFirst/push，在开头插入指定元素
        addLast，将指定元素追加到表的末尾
        getFirst，返回首元素
        getLast，返回末尾元素
        removeFirst/pop，从列表删除并返回第一个元素
        removeLast，从列表删除并返回最后一个元素
        ```
  
        
  
    * **Set**（接口）：体系接口，这个系列添加的元素**都是不重复、无索引的**，一般是**无序（存储顺序不一致）**
      
      **Set集合功能上基本与Collection的API一致**
      
      
      
      * **HashSet**（基于HashMap结构）：添加的元素**无序**、**不重复**、**无索引**
      
        底层采用**哈希表**存储数据
      
        **哈希表：对于增删改查性能都比较好的结构**（JDK8之前为 数组+链表，**JDK8之后为数组+链表+红黑树**）
      
        **哈希值：JDK根据对象的地址，按照某种规则算出来的int类型的数值**
      
        ​	**Object类API：hashCode() -> 返回对象的哈希值** 
      
        对象的**哈希值特点**：
      
        1. **同一个对象多次调用hashCode方法返回的哈希值是相同的**
        2. **默认情况下，不同对象的哈希值是不同的**
      
        HashSet（JDK7 数组+链表+哈希算法）原理：
      
        1. 创建一个默认16长度的数组，数组名为table（默认加载因为0.75，当数组存满到16*0.75=12时，自动扩容，每次扩容都到之前的两倍）
        2. 根据元素的**哈希值和数组长度(16)求余**(结果范围：0-15)，**计算出元素应该存入的位置**（哈希算法）
        3. **元素存入之前判断计算出来的位置是否为null**，为null直接存入
        4. 不为null，也就是**有元素，那么调用equals方法与该元素比较，默认比较元素地址**，想比较属性内容需要重写equals
        5. 一样则不存。不一样则存入数组，**存入方式在JDK7为新元素占据老元素位置，同时新元素会有地址指向老元素**，**相当于形成了一个链表**。**JDK8为新元素挂在老元素下面，也就是老元素指向新元素**
      
        **HashSet（JDK8）原理**：
      
        ​	底层结构：**哈希表**（数组+链表+红黑树的结合体）**当挂在老元素下方的元素过多，形成的链表长度超过8时，自动转为红黑树**（根据哈希值大小排树，小的排左子树，大的排右子树）
      
        **HashSet去重复**：如果**希望Set集合认为两个内容一样的对象是重复的**需要去掉重复，那么**必须重写对象的hashCode()和equals()方法**
      
        > 详情请见：[**为什么重写equals还要重写hashCode？.md**]
      
        
      
        * **LinkedHashSet**（基于LinkedHashMap）：添加的元素**有序(存储和取出的顺序一致)**、不重复、无索引
      
          **底层结构依然是哈希表**，但每个元素多了一个额外的双链表机制（记录下一个元素和上一个元素位置信息）记录存储的位置
      
          
      
      * **TreeSet**（基于TreeMap）：添加的元素**按照大小默认升序排序**、不重复、无索引
      
        底层基于**红黑树**
      
        **TreeSet是一定要排序的**，将元素按照指定规则进行排序，或者默认升序排序（数值为大小，字符串为首字符编码大小，自定义类型无法默认排序）
      
        自定义排序规则：
      
        1. 让自定义类**实现Comparable接口，重写compareTo方法**制定比较规则
        2. 用TreeSet的**有参构造器，传入Comparator接口对应的比较器对象（匿名内部类）**，重写compare制定比较规则，**一般用这个**
        3. 比较规则中，第一个大于第二个，返回正整数，小于为负整数，**相等为0，此时TreeSet认为两者重复，只会保留一个，做到去重复**
        4. 当类内**即有比较规则，又给TreeSet传入了比较器**，那么**默认使用比较器**
      
        TreeSet的不重复机制是利用比较规则，当相等时认为重复去掉，与HashSet不一样，HashSet需要重写equals和hashCode
      
        
  
  * **Map**（双例集合**祖宗接口**）：体系接口，每个元素包含两个值（键值对 key=value）  
  
    格式：{key1=value1，key2=value2，...}
  
    **Map的键：无序，不重复，无索引**，后面重复的键会覆盖前面已经存在的键
  
    Map的值：不做要求
  
    键值对都可以是null
  
    常用API：（体系祖宗API）
  
    ```java
    V put(K key, V value);//添加元素
    V get(Object key);//根据建 获取元素
    V remove(Object key);//根据键 删除键值对
    void clear();//移除所有的键值对元素
    boolean containsKey(Object key);//判断集合是否包含指定的键
    boolean containsValue(Object value);//判断集合是否包含指定的值
    boolean isEmpty();//判断集合是否为空
    int size();//返回集合的长度，也就是键值对的个数
    Set<K> keySet();//获取全部键的Set集合（Set不能直接new对象，底层是用HashSet之类的实现类返回的）
    Collection<V> values();//获取全部值的Collection集合（Collection可以保留重复的值）
    void putAll(Map<? extends K,? extends V> m);//将参数的map集合拷贝一份加到调用者集合里面去（用于合并其他Map）
    Set<Map.Entry<K,V>>entrySet();//获取所有 键值对对象 的集合
    K getKey();//获得键
    V getValue();//获得值
    void forEach(BiConsumer<? super K, ? super V> action);//结合Lambda遍历Map集合
    ```
  
    **遍历Map集合方式**：
  
    1. **键找值（一般用这个）**
  
       先获取Map的全部键的Set集合
  
       然后遍历键的Set集合，通过获取的键提取对应的值
  
       所需API（**keySet**/**get**）
  
    2. 键值对
  
       先把Map集合转为Set集合，Set集合中的每个元素就是**键值对的实体类型**
  
       然后增强for遍历Set集合，然后提取 键 和 值
  
       所需API（entrySet/getKey/getValue）
  
    3. 结合Lambda
  
       内部原理是进行了键值对的遍历方式
  
       结合Lambda省略写法
  
       ```java
       maps.forEach(new BiConsumer<String, Integer>() {
       	@Override
       	public void accept(String key, Integer value) {
       		System.out.println(key+"---->"+value);
       	}
       });
       //Lambda省略
       maps.forEach((k,v)—>System.out.println(k+"---->"+v));
       ```
  
       
  
    * **HashMap**（键是无序，不重复，无索引，与Map一致）(用的最多)
  
      常用API与Map接口一致
  
      **底层原理和HashSet底层原理一模一样，都是哈希表结构**，只是HashMap的每个元素包含两个值而已（Set系列集合底层就是Map实现的，new的Map实现类，只不过Set只要值，不要键而已）
  
      **依赖hashCode和equals方法保证键的唯一**，如果键要存储自定义对象，那么需要重写hashCode和equals方法（内容重复的对象看做是同一个键）
  
      基于哈希表，那么增删改查性能都好
  
      
  
      * **LinkedHashMap**（**键是有序，不重复，无索引**）
  
        有序指存储和取出的元素顺序是一致的
  
        **底层原理依然是哈希表**，只是基于HashMap**多了个双链表记录存储的顺序**
  
        常用API与Map接口一致
  
      
  
    * **HashTable**
  
      **键和值都不能为null**
  
      使用方法和HashMap一致，也就是常用API基本一致
  
      特点：**HashTable是线程安全的**，HashMap线程不安全
  
      底层原理：内部有一个数组 (HashTable$**Entry**) 初始大小为11，临界值为8（11*0.75）超过8就会扩容
  
      
  
      * **Properties**
  
        **继承自HashTable**并实现了Map，**线程是安全的**
        
        用键值对来保存数据，键值对不能为null
        
        常用API与Map一致
        
        Properties用于从xxx.properties文件加载数据到Properties类对象中，进行读取和修改
        
        也就是说，Properties是**从properties配置文件中读取数据的中间桥梁**
        
        
    
    * **TreeMap**（**键是可排序，不重复(键内容要不一致)，无索引**）
    
      可排序指**按照键的数据大小，默认升序排序（只能对键排序）**
    
      底层原理与TreeSet一致（因为TreeSet底层就是TreeMap）
    
      想排序，基本类型自动帮你排，对于自定义类型，要么类实现Comparable接口，重写比较规则，要么**集合传入Comparator对象，重写比较规则**
    
      **根据大小规则，相等就认为是重复的**
    
      
  
  
  
* **集合都是支持泛型的**，在**编译阶段约束**集合只能操作某种数据类型

* 集合（不管是Collection还是Map）**都可以嵌套使用**

* 从集合中找出某个元素并删除时可能会出现一种**并发修改异常问题**

  * **迭代器遍历集合且直接用集合删除元素**时可能会出现
    * 也就是会**出现漏删**，会报错，比如两个连续的需要删除的元素，删除第一个之后，迭代器会后移，跳过了那个连续的元素
    * 解决方法：**用迭代器自带的remove方法**，不要直接用集合自带的remove。迭代器的remove内部有自减操作，会回退。
    * 所以**迭代器遍历就要用迭代器删除元素**
  * **增强for循环遍历集合且直接用集合删除元素**时可能会出现
    * 也是漏删情况，且会报错
    * 解决办法：**没有解决办法**，如果**要遍历删除，那就只有选择其他的方法**
  * **forEach方法**也会报错，因为**内部是增强for原理**，所以也没有解决方法，不能选择这个来遍历删除
  * **for循环遍历**也可能会漏删，但不会报错
    * 解决办法：**倒着删**/**顺着遍历时，内部自己实现自减后退(i--)**



* **集合工具类：Collections**
  * 语法悉知：<? super T> 意为传入的为T的本身或者T的父类
  * 常用API：
    * addAll(Collection<? super T> c，T...elements)：给集合对象批量添加元素
    * shuffle(List<?> list)：打乱list的元素排序
    * sort(List\<?> list)：将list元素按照默认规则排序，不能直接对自定义类型排序，除非实现了Comparable接口
    * sort(List<?> list，Comparator<? super T> c)：将list按照传入的规则排序



* **不可变集合**：不可以被修改的集合（类似于静态数组）

  * 集合的数据项在创建的时候提供，并且**在整个生命周期中都不可以被改变**（添加或更换都不可以）

  * 使用场景：

    * 某个数据不能被修改，把它防御性地拷贝到不可变集合中（也就是只想给你看，但你别动老子的）
    * 集合对象被不可信的库调用时，不可变集合是安全的

  * 创建方式：

    * 首先，对于集合，不管是Collection还是Map，（List Set Map）都可以创建，它们类里面都有一个静态的 of方法

    * ```java
      static <E> List<E> of(E...elements);//创建一个具有指定元素的List对象，也就是不可变集合对象
      static <E> Set<E> of(E...elements);//创建一个具有指定元素的Set对象，也就是不可变集合对象
      static <E> Map<K,V> of(E...elements);//创建一个具有指定元素的Map对象，也就是不可变集合对象
      ```

    * 你可以把数据取出来自己处理，但是不可以在集合里面处理，改变不了集合

    * 不能添加，不能修改，不能删除





## 24. Java的泛型

* 作用：**在编译阶段约束操作的数据类型，并进行检查**
* 格式：<数据类型 E >，泛型只支持引用数据类型，基本数据类型请用**包装类**
* 把出现泛型变量的地方转换成真实数据类型
* **集合体系**的所有接口和实现类都是支持泛型的
* 好处：统一数据类型。把运行时的问题提前到了编译期间，**避免了强制类型转换可能出现的异常，因为编译阶段就已经确定了类型**
* 可以在很多地方定义：
  * 类后面：**泛型类**
    * 定义类的同时定义了泛型的类
    * 格式：修饰符 class 类名<泛型变量（E T K V等）>{}
    * 编译阶段指定数据类型，类似于集合
  * 方法申明上：**泛型方法**
    * 定义方法的同时定义泛型
    * 格式：修饰符 <泛型变量> 方法返回值 方法名(形参列表) {}
    * 方法中可使用泛型接收一切实际类型的参数，更具通用性
  * 接口后面：**泛型接口**
    * 定义接口同时定义了泛型
    * 格式：修饰符 interface 接口名称 <泛型变量> {}
    * 泛型接口可以**让实现类选择当前功能需要操作的数据类型**
    * 实现类**在实现接口的时候传入自己操作的数据类型，重写的方法都是针对于该类型的操作**
* 泛型的通配符：**？**，在**使用泛型时**，**代表一切类型**，也就是**在使用泛型时，用通配符，表示一切类型都可以传过来**
  * 泛型变量是在**定义泛型**时使用，**比如定义方法、接口或者类时，泛型变量约束了它们只能操作定义时的类型**
* 泛型的上下限：
  * **泛型上限**：? extends 子类类名（只能是这个类或者它的**子类**）
  * **泛型下限**：? extends 父类类名（只能是这个类或者它的**父类**）



## 25. Java的可变参数

用在形参中，可以接受多个数据

格式：`数据类型...参数名称`

* 作用：参数参数非常灵活，**传输的参数个数为0~n个，也可以传输一个数组**
* 原理：可变参数的参数名称在对应的方法内**本质上就是一个数组**，**参数名称就是数组名**
* 注意：
  * 一个形参列表**只能有一个可变参数**
  * **可变参数必须放在形参列表的最后面**



## 26. Java的Stream流

得益于Java8的Lambda函数式编程，引入的全新概念

目的：**用于简化集合和数组操作的API**（结合Lambda表达式）

思想：

1. 先得到的集合或者数组的Stream流（也就是得到了一条**处理这个集合或者数组的流水线传送带**）
2. 将集合的元素放到这个流水线上
3. 然后用Stream的API来操作这些元素



Stream的三类方法：

*  获取方法：创建一条流水线，并把数据放在流水线上准备进行操作

  * 集合获取Stream：

    （Collection接口）默认stream方法

    ```java
    default Stream <E> stream();//获取当前集合对象的Stream对象
    ```

    Map接口获取Stream：

    ```java
    Map<String,Integer> maps = new HashMap<>();
    
    //获取键的流
    Stream<String,Integer> keyStream = maps.keySet().stream();
    //获取值的流
    Stream<String,Integer> valueStream = maps.values().stream();
    //获取键值对（整体的）流
    Stream<Map.Entry<String,Integer>> keyValueStream = maps.entrySet().stream();
    ```

  * 数组获取Stream：

    ```java
    //用Arrays工具类
    public static <T> Stream<T> stream(T[] array);//获取当前数组的Stream流
    
    //用Stream工具类
    public static <T> Stream<T> of(T...values);//获取当前数组/可变数据的Stream流
    ```

    

* 中间方法：流水线上的操作，一次操作完成之后，还可以继续完成其他的操作

  常用中间操作API：

  ```java
  Stream<T> filter(Predicate<? super T> predicate);//对流中的数据进行过滤，条件对象用Lambda简化，这个方法会遍历集合或数组对象
  例如：filter(s -> s.startsWith("王"));
  Stream<T> limit(long maxSize);//获取前几个元素
  Stream<T> skip(long n);//跳过前几个元素
  Stream<T> distinct();//去除流中重复的元素，需要依赖hashCode和equals方法
  static <T> Stream<T> concat(Stream a,Stream b);//将a和b合并为一个流（不同类型的两个流合并为一个流，最终应该合并为Object类型流）
  Stream<R> map(Function<? super T, ? extends R> mapper);//加工元素方法(第一个泛型为集合原始元素类型 第二个泛型为加工后的最终目标，可以为自定义类型)
  max、min、sorted
  ```

  **中间方法返回的流可以继续进行操作**

  

* 终结方法：流水线上的最后一个方法，一个Stream流只能有一个终结方法

  ```java
  void forEach(Consumer action);//对流的每个元素遍历并操作
  long count();//返回此流中的元素个数
  ```

  **终结之后的流无法再继续进行操作**

【注】在Stream流中，**无法直接对集合或数组中的数据进行修改，相当于这是拷贝过来的，进行数据分析**



Stream流的收集方法：

将一个Stream流中的数据收集起来，重新放在一个集合或者数组中去（也就是将传送带上的货物收集包装起来）（平常开发时，不可以直接传输Stream流对象，所以将流的数据收集起来重新放在一个集合或者数组中去再传输）

```java
R collect(Collector collector);//开始收集Stream流，指定收集器
```

* **Collectors工具类：提供具体的收集方法**

  ```java
  Collectors.toList();//把元素收集到List集合中去
  注：Java-16开始 可以直接用Stream对象调用toList方法收集为一个List集合 且获得的集合是不可变的
  Collectors.toSet();//把元素收集到Set集合中去（会去重复）
  Collectors.toMap();//把元素收集到Map集合中去
  
  收集到数组中：
  Stream对象.toArray();//把元素收集到一个数组中
  注：一般情况下获取的数组是Object类型的，但是我们也可以直接获取对应类型的数组
      只需要 向toArray方法传入匿名内部类IntFunction<T[]>，T为想要的类型
      这样既可直接获得对应类型的数组
      根据Lambda简化
      所以可以直击这样写：Stream对象.toArray(String[]::new);//也就是方法引用 以后都这样写了 方便简单
  ```

【注】一条Stream流收集完了之后就会被关闭，相当于一个终结方法。所以**想再次收集的话，就需要重新获取一条新的Stream流**





## 27. Java的异常处理

异常：**在”编译“或者”执行“的过程中可能出现的问题**（语法错误不是，那是水平问题）

* 比如：数组索引越界、空指针异常、日期格式化异常等

异常的出现，如果没有提前处理，那么程序就会退出JVM虚拟机而终止

研究异常、避免异常、提前处理异常、体现程序的安全性健壮性



异常体系：

**Throwable**：祖宗接口

* **Error**：系统级别错误，比如JVM退出等。代码无法控制，我们**无法处理**
* **Exception**：**异常类**。表示程序本身**可以处理的问题**
  * **RuntimeException**及其子类：运行时异常，编译时不会报错（空指针，数组越界）
  * 除RuntimeException之外的所有的异常：编译时异常，编译期必须处理，否则无法通过编译（日期格式化异常）



编译时异常：没有继承RuntimeException等相关类，在编译成class文件时**必须要处理的异常**。

运行时异常：继承RuntimeException或其子类，编译成class文件不需要处理，且不会报错，但在运行字节码文件时可能会出现的异常



常见运行时异常：（水平型异常）

* 数组索引越界异常：ArrayIndexOutOfBoundsException
* 空指针异常：NullPointerException（调用空指针的变量的功能就会报错）
* 数学操作异常：ArithmeticException
* 类型转换异常：ClassCastException
* 数字转换异常：NumberFormatException

常见编译型异常：（技术型异常）

* 日期解析异常：ParseException



**异常的默认处理机制**：（简而言之，**打印异常信息，干掉程序进程**）

1. 默认在出现异常的代码那里自动创建一个异常对象
2. 异常从方法中出现的点抛出给调用者，调用者最终抛给JVM
3. JVM接收到异常对象之后，先在控制台直接输出**异常栈**（先指出是什么异常，再指出异常的位置）的信息数据
4. 直接从当前执行的异常点干掉当前程序
5. 后续代码无法执行，程序已经死亡



**编译时异常处理方式**：

一、出现异常直接抛出给调用者，调用者继续抛出去，最终抛给了JVM（也就是默认处理）

* throws：用在方法上，将方法内部的异常抛出去给本方法的调用者处理

  ```java
  //格式
  方法名(参数) throws 异常1,异常2,异常3.. {}
  //虽然抛的异常多，但是最终还是只会抛出一个异常，因为其他的都没机会异常，看谁先出现
  
  //抛多个异常的规范处理：统一按Exception抛出
  方法名(参数) throws Exception {}
  ```

  

二、出现异常自己捕获处理（**发生异常的方法自己独立完成异常的处理**，程序还可以继续运行）

* try...catch...：监视捕获异常，用在方法内部，将方法内部的异常直接捕获处理

  ```java
  //格式
  try{
      
  } catch(异常类型1 变量) {
      
  } catch(异常类型2 变量) {
      
  }...
  //建议规范格式
  try{
      //所有的执行代码
  } catch(Exception e) {//捕获处理一切的异常类型
      e.printStackTrace();//打印异常栈信息
  }
  ```

  

三、出现异常throws抛出给调用者，调用者捕获处理

* 底层异常抛出去给最外层，最外层集中处理（框架使用）



**运行时异常处理方式**：

* 可以不处理，因为编译时不报错
* 建议抛出由最外层捕获处理



**自定义异常**：（Java无法为世界上的所有异常都提供异常类）

1. 定义一个异常类继承Exception（编译型异常）/继承RuntimeException（运行时异常）
2. 重写所需构造器
3. 在出现异常的地方用**throw new 自定义异常对象**抛出
   * throw：用在方法内部，创建异常对象并抛出

错误很严重就用编译型异常，不是很严重就定义成运行时异常





## 28. Java的日志

日志：记录程序运行过程中的信息

日志技术：可以将系统执行的信息选择性的记录到指定位置（控制台、文件、数据库）



Java日志体系结构：

* 日志规范接口：
  * Commons Logging（JCL）
  * Simple Logging Facade for Java（slf4j）
* 日志实现框架：
  * Log4J
  * JUL：官方JDK里的实现
  * **Logback**：目前主流，性能较好（相当于Log4j的升级版）



Logback日志框架：

分为三个模块：

* logback-core：为其他两个模块奠定基础
* logback-classic：实现了SLF4J API，可视为log4j的升级版本
* logback-access：与Tomcat和Jetty等Servlet容器集成，提供HTTP访问日志

依赖坐标：

```xml
<!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-classic -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.4.4</version>
    <scope>test</scope>
</dependency>
```

核心配置文件：logback.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!--定义日志文件的存储地址 勿在 LogBack 的配置中使用相对路径-->
    <property name="LOG_HOME" value="D:/logback/log" />
    <!-- 控制台输出 -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <!-- 日志输出编码 -->
        <Encoding>UTF-8</Encoding>
        <layout class="ch.qos.logback.classic.PatternLayout">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
            </pattern>
        </layout>
    </appender>
    <!-- 按照每天生成日志文件 -->
    <appender name="FILE"  class="ch.qos.logback.core.rolling.RollingFileAppender">
        <Encoding>UTF-8</Encoding>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志文件输出的文件名-->
            <FileNamePattern>${LOG_HOME}/myApp.log.%d{yyyy-MM-dd}.log</FileNamePattern>
            <MaxHistory>30</MaxHistory>
        </rollingPolicy>
        <layout class="ch.qos.logback.classic.PatternLayout">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
            </pattern>
        </layout>
        <!--日志文件最大的大小-->
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <MaxFileSize>1MB</MaxFileSize>
        </triggeringPolicy>
    </appender>
 
    <!-- 日志输出级别 -->
    <root level="ALL">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="FILE" />
    </root>
 
</configuration>
```

创建logback日志对象：

```java
public static final Logger LOGGER = LoggerFactory.getLogger("产生日志对象的类名.class");
```

日志级别：TRACE < DEBUG < WARN < ERROR

默认为DEBUG，且只这些都是对应的方法（可以用LOGGER调用）、



## 29. Java的File类

File类代表操作系统中的文件对象（包括文件和文件夹）

File类功能：

* 定位文件
* 获取文件本身信息
* 删除文件
* 创建文件（或者是创建文件夹）
* **无法读写文件内容**



构造方法：

```java
File(String pathname);//最常用的，填入文件路径即可
文件路径："D:\\xxxx\\xxx.jng"(双反斜杆防止转义字符) 或者："D:/xxxx/xxx.jng" 或者用File.separator自适应生成分隔符，文件路径用字符串拼接
File(String parent, String child);//从父路径名字符串和子路径名字符串创建文件对象
File(File parent, String child);//根据父路径对应的文件对象和子路径字符串创建文件对象
```

支持**绝对路径**（从盘符开始 D:/xxx)、相对路径（一般用于定位模块中的文件，相对到工程文件下，也就是**从你想使用相对路径的项目工程文件开始**）

File可以是文件夹，取它的大小是没有意义的，文件夹的大小无法取得，文件大小可以，一般定位文件夹用于判断路径是否存在

File封装的对象仅仅是一个路径名，可以是存在的，也可以是不存在的（不存在的话可以后面自己根据这个自己创建出来）



常用API：

```java
boolean isDirectory();//判断此文件对象是否为文件夹（Directory，也就是目录）（D盘是一个文件夹 D:\）
boolean isFile();//判断文件对象是否为文件
boolean exists();//判断文件对象是否存在
String getAbsolutePath();//返回文件对象的绝对路径字符串（必须获取绝对路径）
String getPath();//返回文件对象的路径名字符串（可以是绝对路径，也可以是相对路径，看文件对象封装的是什么路径）
String getName();//返回文件对象代表的文件或文件夹的名称
long lastModified();//返回文件最后被修改的毫秒值
long length();//返回文件对象的大小（字节大小）

//File类创建文件API
boolean creatNewFile();//将封装的文件路径代表的文件创建出来，已存在就创建失败（一般不用这个，因为在IO流中，写文件内容时会自动帮我们创建出来）
boolean mkdir();//将封装的文件路径创建出来，创建一级文件夹，不能创建多级文件夹（只能D:/aaa，不能D:/aaa/aa）
Boolean mkdirs();//创建多级文件夹，一般用这个（可以D:/aaa/aa）

//删除文件API
boolean delete();//删除这个文件对象代表的文件或者空文件夹
//delete方法直接删除，不会给你放在回收站
//删除一个文件，文件被占用依然可以直接删除
//默认只能删除空文件夹，不能删除非空文件夹（可以自己写算法删除非空文件夹）

//遍历文件API
String[] list();//获取文件对象代表的文件夹下所有的“一级文件名称”到一个字符串数组中
File[] listFiles();//获取文件对象代表的文件夹下所有的“一级文件对象”到一个文件对象数组中（一般用这个）
//遍历的调用者必须是文件夹，为文件时返回null
//调用的文件夹必须存在，不存在返回null
//有内容时，将一级文件的绝对路径封装为File类对象放到File数组返回，有隐藏文件也会被找到并放进去
```



文件搜索：递归方式，从文件夹中搜索想要的文件或者文件夹 

```java
//1.传入文件夹 和 想要找的文件
public static void searchFile(File dir, String fileName) {
    //2.判断传入的是否合法 且是否是文件夹
    if (dir != null && dir.isDirectory()) {
        //3.提取当前文件夹下的所有一级文件到一个文件对象数组中
        File[] files = dir.listFiles();
        //4.判断当前文件夹是否存在一级文件（不为空文件夹且不为null）
        if (files != null && files.length > 0) {
            //5.遍历所有一级文件
            for (File file : files) {
                //6.判断遍历的当前一级文件是 文件 还是 文件夹
                if (file.isFile()) {
                    //7.是文件，那么判断这个文件是不是想要找的
                    if (file.getName().contains(fileName)) {
                        //找到之后的操作逻辑
                    }
                } else {
                    //7.是文件夹，那么以这个文件夹递归继续寻找
                    searchFile(file, fileName);
                }
            }
        }
    }
}
```

拓展知识：启动.exe文件

```java
Runtime r = Runtime.getRuntime();
r.exec(file.getAbsolutePath());
```





## 30. Java的IO流

IO流前置知识：**字符集**

给人类字符进行编号存储，这些**编号规则**就是字符集

* ASCII字符集：**一个字节一个字符**，一个字节八位（包含英文，数字，符号，都是一个字节）
* GBK字符集：**兼容ASCII字符集**，同时**一个中文汉字两个字节**（包含几万汉字，繁体，部分日韩文字）
* Unicode字符集：**兼容ASCII字符集**，**常用UTF-8编码，一个中文三个字节**（全世界通用，也叫统一码）

**编码前和编码后字符集要一致，不然会中文乱码**

编码解码操作：

```java
String编码：
byte[] getBytes();//使用平台默认的字符集将String编码为一系列字节，将结果存储到新的字节数组中
byte[] getBytes(String charsetName);//使用指定字符集将string编码为一系列字节，将结果存储到新的字节数组中

String解码：
String(byte[] bytes);//使用平台默认的字符集将指定字节数组解码构造为新的String
String(byte[] bytes, String charsetName);//使用指定字符集将将指定字节数组解码构造为新的String
```



IO流：也叫输入输出流

input - 输入、读

output- 输出、写

按流中**数据最小单位划分**：

* **字节流：操作所有类型文件，但主要是音视频文件**
* **字符流：只能操作纯文本文件，也就是java文件，txt文本等**

按流的传递方向划分：

* 输入流
  * 字符输入
  * 字节输入
* 输出流
  * 字符输出
  * 字节输出



IO流体系：

* 字节流

  * **InputStream**：字节输入流（抽象类）

    * **FileInputStream**：文件字节输入流

      作用：将磁盘文件以字节形式读取到内存中

      构造方法：

      ```java
      public FileInputStream(File file);//创建字节输入流与文件对象代表的源文件接通
      public FileInputStream(String pathname);//创建字节输入流与文件路径代表的源文件接通（内部帮你new File）
      ```

      方法：

      ```java
      public int read();//每次连续读取一个字节返回，没有可读的或直到全部读取完毕返回-1
      //读取中文时会截断导致乱码
      
      public int read(byte[] buffer);//每次读取一个字节数组返回读取的长度，没有可读的或直到全部读取完毕返回-1
      //用同一个固定长度的字节数组读取，可能会有污染数据现象
      //解决方法就是读取完毕后“倒掉”，也就是读取多少先使用掉清空字节数组，再去读取
      //依然会有读取截断中文的乱码现象
      
      //解决字节流读取截断中文乱码现象：一次性读取完所有字节（与文件大小一样大的字节数组）
      public byte[] readAllBytes() throws IOException;//将文件所有字节装入字节数组返回，不支持超大文件
      ```

      

  * **OutputStream**：字节输出流（抽象类）

    * **FileOutputStream**：文件字节输出流

      作用：将内存数据以字节形式写到磁盘文件中

      构造方法：

      ```java
      public FileOutputStream(File file);//创建字节输出流与文件对象代表的源文件接通，构造都是不需要文件存在，不存在自动创建，存在则覆盖
      public FileOutputStream(File file, boolean append);//创建字节输出流与文件对象代表的源文件接通，是否追加数据，不追加则覆盖
      public FileOutputStream(String pathname);//创建字节输出流与文件路径代表的源文件接通
      public FileOutputStream(String pathname, boolean append);//创建字节输出流与文件路径代表的源文件接通，是否追加数据，不追加则覆盖
      ```

      方法：

      ```java
      public void write(int a);//写一个字节出去，不可以写单个中文，会乱码
      public void write(byte[] buffer);//写一个字节数组出去（将中文转为字节数组再写/实现写出去的数据可以换行：write("\r\n".getBytes())）
      public void write(byte[] buffer, int pos, int len);//写一个字节数组的部分出去
      
      //写数据必须要刷新数据，因为这个类自带了一些缓冲区，防止写的数据还在缓冲区没有真的写到文件中去
      flush();//刷新流，让数据生效，刷新完可以继续写数据
      close();//关闭流，释放资源，在关闭之前会先自动刷新流，关闭后不可以再写数据
      ```
      
    
  * 文件拷贝实现：字节输入流和字节输出流可以操作一切文件，适合文件拷贝。拷贝前后编码要一致

    ```java
    public void FileCopy(String SourceFile, String Des) {
    	try {
            //1.与目标文件接通
            InputStream is = new FileInputStream(SourceFile);
            //2.与目的路径接通
            OutputStream os = new FileOutputStream(Des);
            //3.转移数据 -- 也可以直接用输入流的readAllBytes方法，然后再全部输出
            byte[] buffer = new byte[1024];
            int len = 0;
            while((len = is.read(buffer)) != -1) {
                os.write(buffer,0,len);
            }
            os.flush();
            os.close();
            System.out.println("Copy 完成");
        } catch(Exception e) {
            e.printStackTrace();
        } 
    }
    ```

    

  * 资源释放：

    * try-catch-finally：在异常处理时提供finally块执行必要操作，比如IO流的资源释放

      特点：**finally内的语句一定会执行**，除非JVM退出，且都有return语句的话，try或catch内的return会将返回值临时存储到一个内存区内，然后**finally的return的返回值会覆盖掉这个临时return值并返回**

      格式：**try{}catch{}finally{}**

    * try-with-resource：

      格式：jdk8之前为 ：**try(只能放置定义资源对象){}catch{}..**（资源对象用完后会自动关闭，会自动调用该对象的close方法，即使出现异常也会关闭）

      【注】资源对象：实现了Closeable/AutoCloseable接口的对象，重写close方法

      jdk9之后，可以在try外部new资源对象，然后传入资源对象引用：try(资源1引用;资源2引用..){}catch{}..（不常用）



* 字符流：按字符读取，更适合读取中文、文本

  * **Reader**：字符输入流（抽象类）
    * **FileReader**：文件字符输入流
    
      作用：将文件数据以字符的形式读取到内存
    
      构造方法：
    
      ```java
      public FileReader(File file);//创建字符输入流与文件对象代表的源文件接通
      public FileReader(String pathname);//创建字符输入流与文件路径代表的源文件接通
      ```
    
      方法：
    
      ```java
      public int read();//每次读取一个字符返回字符的编码，没有字符可读则返回-1（编码一致，读取中文不会乱码）
      public int read(char[] buffer);//每次读取一个字符数组返回字符数组长度（按字符个数算长度），没有字符可读则返回-1
      ```
    
      
    
  * **Writer**：字符输出流（抽象类）
    * **FileWriter**：文件字符输出流
    
      作用：将内存数据以字符形式写到磁盘文件
    
      构造方法：
    
      ```java
      public FileWriter(File file);//创建字符输出流与文件对象代表的源文件接通，写文件不需要存在，没有就创建，有就覆盖
      public FileWriter(File file,boolean append);//创建字符输出流与文件对象代表的源文件接通，可以追加数据
      public FileWriter(String filepath);//创建字符输出流与文件路径代表的源文件接通，文件不需要存在，没有就创建，有就覆盖
      public FileWriter(String filepath,boolean append);//创建字符输出流与文件路径代表的源文件接通，可以追加数据
      ```
    
      方法：
    
      ```java
      void wirte(int c);//写一个字符，虽然是int，但是可以写字符和字符编码
      void wirte(char[] charbuffer);//写一个字符数组
      void wirte(char[] charbuffer,int begin,int len);//写字符数组的一部分
      void wirte(String str);//写一个字符串
      void wirte(String str,int begin,int len);//写字符串的一部分
      
      //写数据必须要刷新数据，因为这个类自带了一些缓冲区，防止写的数据还在缓冲区没有真的写到文件中去
      flush();//刷新流，让数据生效，刷新完可以继续写数据
      close();//关闭流，释放资源，在关闭之前会先自动刷新流，关闭后不可以再写数据
      ```
    
      





