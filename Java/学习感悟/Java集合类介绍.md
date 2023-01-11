# Java集合类介绍

**Collection**

-> List

​	--> ArrayList

​	--> LinkedList

​	--> Vector

-> Set

​	--> HashSet

​		 ---> LinkedHashSet

​	--> SortedSet

​		 ---> TreeSet

**Map**

-> HashMap

-> HashTable

​	--> Properties

-> WeakHashMap

-> TreeMap





Vector : 基于Array的List，其实就是封装了Array所不具备的一些功能方便我们使用，它不可能走入Array的限制。性能也就不可能超越Array。所以，在可能的情况下，我们要多运用Array。另外很重要的一点就是Vector“sychronized”的，这个也是Vector和ArrayList的唯一的区别。

ArrayList：同Vector一样是一个基于Array上的链表，但是不同的是ArrayList不是同步的。所以在性能上要比Vector优越一些，但是当运行到多线程环境中时，可需要自己在管理线程的同步问题。

LinkedList：LinkedList不同于前面两种List，它不是基于Array的，所以不受Array性能的限制。它每一个节点（Node）都包含两方面的内容：1.节点本身的数据（data）；2.下一个节点的信息（nextNode）。所以当对LinkedList做添加，删除动作的时候就不用像基于Array的List一样，必须进行大量的数据移动。只要更改nextNode的相关信息就可以实现了。这就是LinkedList的优势。

 

List总结：

1. 所有的List中只能容纳单个不同类型的对象组成的表，而不是Key－Value键值对。例如：[tom,1,c ]；

2. 所有的List中可以有相同的元素，例如Vector中可以有 [ tom,koo,too,koo ]；

3. 所有的List中可以有null元素，例如[ tom,null,1 ]；

4. 基于Array的List（Vector，ArrayList）适合查询，而LinkedList（链表）适合添加，删除操作。





HashSet：虽然Set同List都实现了Collection接口，但是他们的实现方式却大不一样。List基本上都是以Array为基础。但是Set则是 在HashMap的基础上来实现的，这个就是Set和List的根本区别。HashSet的存储方式是把HashMap中的Key作为Set的对应存储项。看看 HashSet的add（Objectobj）方法的实现就可以一目了然了。

```java
public boolean add(Object obj) {
	return map.put(obj, PRESENT) == null;
}
```

这个也是为什么在Set中不能像在List中一样有重复的项的根本原因，因为HashMap的key是不能有重复的。

LinkedHashSet：HashSet的一个子类，一个链表。

TreeSet：SortedSet的子类，它不同于HashSet的根本就是TreeSet是有序的。它是通过SortedMap来实现的。

 

Set总结：

1. Set实现的基础是Map（HashMap）；

2. Set中的元素是不能重复的，如果使用add(Object obj)方法添加已经存在的对象，则会覆盖前面的对象





HashTable： 实现一个映象，所有的键必须非空。为了能高效的工作，定义键的类必须实现hashcode()方法和equal()方法。这个类 是前面java实现的一个继承，并且通常能在实现映象的其他类中更好的使用。

HashMap： 实现一个映象，允许存储空对象，而且允许键是空（由于键必须是唯一的，当然只能有一个）。

WeakHashMap： 实现这样一个映象：通常如果一个键对一个对象而言不再被引用，键/对象对将被舍弃。这与HashMap形成对照，映象 中的键维持键/对象对的生命周期，尽管使用映象的程序不再有对键的引用，并且因此不能检索对象。

TreeMap： 实现这样一个映象，对象是按键升序排列的。



Map映射特点：

映射与集或列表有明显区别，映射中每个项都是成对的。映射中存储的每个对象都有一个相关的关键字（Key）对象，关键字决定了对象在映射中的存储位置，检索对象时必须提供相应的关键字，就像在字典中查单词一样。关键字应该是唯一的。

关键字本身并不能决定对象的存储位置，它需要对过一种散列(hashing)技术来处理，产生一个被称作散列码(hash code)的整数值，

散列码通常用作一个偏置量，该偏置量是相对于分配给映射的内存区域起始位置的，由此确定关键字/对象对的存储位置。理想情况下，散列处理应该产生给定范围内均匀分布的值，而且每个关键字应得到不同的散列码。

