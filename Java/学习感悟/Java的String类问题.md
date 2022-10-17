# Java的String类问题

* 问题一：以下程序结果，并解释。

  ```java
  public void main(String[] args) {
  	String s1 = new String("abc");
      String s2 = "abc";
      System.out.println(s1 == s2);
  }
  ```

  结果为：false

  * **new创建的String对象变量是放在了堆内存区**
  * 直接用**引号“”包裹创建的字符串，会存在于java的字符串常量池**，Java8在堆内存的另一片特殊区域，叫元空间（metaSpace），反正与变量的地址不一样
  * == 比较两个引用，比较的是地址
  * 这两个地方的地址是不一样的
  * new的方式，相当于创建了两个对象
  * 引号方式只创建了一个，且在字符串常量池

* 问题二：

  ```java
  public void main(String[] args) {
  	String s1 = "abc";
      String s2 = "ab";
      String s3 = s2 + "c"
      System.out.println(s1 == s3);
  }
  ```

  结果为：false

  * 创建了两个字符串常量s1 s2
  * s3是变量s2（s2是变量，值为常量，在常量池）加上再次创建的一个常量，只要不是引号直接创建的字符串，那么它就只会在堆内存
  * s1在字符串常量池
  * 所以地址不相同

* 问题三：

  ```java
  public void main(String[] args) {
  	String s1 = "abc";
      String s2 = "a" + "b" + "c";
      System.out.println(s1 == s2);
  }
  ```

  结果为：true

  * 因为java存在一个**编译优化机制**
  * 这个机制，就是为了提升性能，编译时，直接将"a" + "b" + "c"转为了"abc"
  * 因为这个运算后的字符串，编译器能直接看出来已经存在了，那就直接转换成字符串常量池里现有的就行了，减少再次创建的开销，提升性能
  * 因为是同一个字符串常量对象，所以地址一样