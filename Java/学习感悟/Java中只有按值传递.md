# Java中只有按值传递

> Java 总是采用`按值调用`。也就是说，方法得到的是所有参数值的一个拷贝，即，方法不能修改传递给它的任何参数变量的内容。



## swap场景

> 要求写一个函数交换`int`类型的`a`和`b`的值

```java
public static void swap(int a, int b){
        int tmp = a;
        a = b;
        b = tmp;
}
```

在 `swap()` 方法中，a、b 的值进行交换，并不会影响到 A、B。因为a、b 中的值，只是从 A、B 复制过来的。也就是说，a、b 相当于 A、B 的副本，副本的内容无论怎么修改，都不会影响到原件本身。



### Q#1：如果将上面的`int`类型转变为`Integer`，`swap(Integer a, Integer b)`会不会实现交换功能呢?

`Integer`使用`final`修饰的`int`进行存储。`final`修饰的变量不能被重新赋值，所以操作参数传递变量时，实际上是操作`变量对象的副本`（Java中的包装类型都是默认使用这种方式实现的，使用拷贝副本的方式提升效率和减少内存消耗）



### Q#2：如果换作是数组呢？

> 场景：改变传入的数组中的某个值

```java
public static void main(String[] args) {
    int[] arr = { 1, 2, 3, 4, 5 };
    System.out.println(arr[0]);
    change(arr);//传入数组，目的是想让其改变数组内的值
    System.out.println(arr[0]);
}

public static void change(int[] array) {
    // 将数组的第一个元素变为0
    array[0] = 0;
}
```

这里方法`array`是数组对象的引用`arr`的拷贝，而**不是对象本身的拷贝**，因此， `array` 和 `arr` 指向的是同一个数组对象



### Q#3：如果换做是一般对象呢？

> 场景：交换两个对象内的属性

```java
class User {
    private String name;

    public User(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return name;
    }
}
----------------------------------------
public class test{
		public static void main(String[] args) {
        User A = new User("ali");
        User B = new User("bd");
        System.out.println("交换前name：" + A + "-->" + B);
        swap(a,b);
        System.out.println("交换后name：" + A + "-->" + B);
    }

    private static void swap(User a, User b) {
        User tmp = a;
        a = b;
        b = tmp;
    }
}
```

运行后的结果：

```java
交换前name：ali-->bd
交换后name：ali-->bd
```

由此可见，是没有交换成功的。

原因：这只是交换了两个对象引用的副本指向的内容

* 交换前：

  ![img](https://img-blog.csdnimg.cn/20201019002523798.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDQ3MTQ5MA==,size_16,color_FFFFFF,t_70#pic_center)

* 交换后：

  ![img](https://img-blog.csdnimg.cn/20201019002637731.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDQ3MTQ5MA==,size_16,color_FFFFFF,t_70#pic_center)

可见是**交换了拷贝后的副本所指向的内容**，原对象本身并没有被交换



### Q#4：那么到底该如何实现交换两个变量的值呢？

#### 方式一：用容器（数组，或者集合）

例如：

```java
public class test{
    public static void main(String[] args) {
        int[] arr = {2, 3};
        int A = arr[0];
        int B = arr[1];
        swap(arr, 0, 1);
        A = arr[0];
        B = arr[1];
        System.out.println(A);
        System.out.println(B);
    }

    public static void swap(int[] arr, int pos1, int pos2){
        int tmp = arr[pos1];
        arr[pos1] = arr[pos2];
        arr[pos2] = tmp;
    }
}

```

也就是，想要交换之前，我们需要将目标装入一个容器内，然后将引用指向的元素交换



#### 方式二：反射

例如：

```java
public static void swap(Integer a, Integer b) throws Exception {
    Field field = Integer.class.getDeclaredField("value");
    field.setAccessible(true);   //设置可以访问成员的私有不可变的变量
    Integer tmp = new Integer(a.intValue());//用a的整数值进行构造
    field.set(a, b.intValue());//将a的值设置为b的整数值
    field.set(b, tmp.intValue());//将b的值设置为先前a的整数值
}
```

