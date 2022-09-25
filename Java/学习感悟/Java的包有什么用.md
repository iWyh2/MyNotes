# Java的包有什么用？

* 包是用来**分门别类地管理类**的，类似于文件夹的东西

* **建包**利于管理和维护程序，**必须在程序的第一行**

  ```java
  //package 域名倒写.技术名称
  如：
  package com.wyh.domain
  ```

* 相同包下的类可以相互之间直接访问

* **不同包之下的类的访问**需要**导包**

  ```java
  //improt 包名.类名
  import com.wyh.domain.User
  ```

* 要访问其他包的相同类名的类，只能导一个的包，访问其他的相同类名的类要用**全名访问**

  ```java
  import com.wyh.domain.User
  ...
  com.wyh.POJO.User user = new com.wyh.POJO.User();
  ```

  