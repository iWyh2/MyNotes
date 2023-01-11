# ArrayList和LinkedList的区别

ArrayList 是基于**数组**实现的

LinkedList 是基于**双向链表**实现的



ArrayList 在新增和删除元素时，因为涉及到数组复制，所以效率比 LinkedList 低

在遍历的时候，ArrayList 的效率要高于 LinkedList



ArrayList 是基于动态数组实现的**非线程安全**的集合

当底层数组满的情况下还在继续添加的元素时，ArrayList则会执行扩容机制扩大其数组长度

ArrayList查询速度非常快，使得它在实际开发中被广泛使用

美中不足的是插入和删除元素较慢，同时它并不是线程安全的

ArrayList使用一个内置的数组来存储元素，这个**数组的起始容量是10**，当数组需要增长时，新的容量按如下公式获得：新容量 = 旧容量×1.5 + 1，也就是说每一次容量大概会增长50%

扩容机制：通过扩容机制判断原数组是否还有空间，若没有则重新实例化一个空间更大的新数组，把旧数组的数据拷贝到新数组中，先判断下标是否越界，再扩容。若插入的下标为i，则通过复制数组的方式将i后面的所有元素，往后移一位，新数据替换下标为i的旧元素

【注】实例化ArrayList时传入了初始长度，那么就默认那么长，不会扩容



LinkedList 是基于双向链表实现的非线程安全的集合，它是一个链表结构，不能像数组一样随机访问，必须是每个元素依次遍历直到找到元素为止。其结构的特殊性导致它查询数据慢
查询时先判断元素是靠近头部，还是靠近尾部，然后再查询。

在LinkedList中有一个私有的内部类，定义如下：

```java
private static class Entry {   
         Object element;   
         Entry next;   
         Entry previous;   
     }   
```

LinkedList中的每一个元素中还存储了它的前一个元素的索引和后一个元素的索引



如果要使用线程安全的集合使用：
Vector 的数据结构和使用方法与ArrayList差不多。最大的不同就是**Vector是线程安全的**。几乎所有的对数据操作的方法都被synchronized关键字修饰。synchronized是线程同步的，当一个线程已经获得Vector对象的锁时，其他线程必须等待直到该锁被释放。从这里就可以得知Vector的性能要比ArrayList低。
**ArrayList和LinkedList都不是线程安全的**，小并发量的情况下可以使用Vector，若并发量很多，且读多写少可以考虑使用CopyOnWriteArrayList
因为CopyOnWriteArrayList底层使用ReentrantLock锁，比使用synchronized关键字的Vector能更好的处理锁竞争的问题。



**ArrayList和LinkedList的区别如下：**

1. **ArrayList的实现是基于数组，LinkedList的实现是基于双向链表。**

2. **对于随机访问，ArrayList优于LinkedList，ArrayList可以根据下标以O(1)时间复杂度对元素进行随机访问。而LinkedList的每一个元素都依靠地址指针和它后一个元素连接在一起，在这种情况下，查找某个元素的时间复杂度是O(n)**

3. **对于插入和删除操作，LinkedList优于ArrayList，因为当元素被添加到LinkedList任意位置的时候，不需要像ArrayList那样重新计算大小或者是更新索引。**

4. **LinkedList比ArrayList更占内存，因为LinkedList的节点除了存储数据，还存储了两个引用，一个指向前一个元素，一个指向后一个元素。**