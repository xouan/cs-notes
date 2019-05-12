面试题汇总

https://blog.csdn.net/linzhiqiang0316/article/details/80473906

1 基础知识

1. ==和equals的区别

- 对于基本类型，== 判断两个值是否相等，基本类型没有 equals() 方法。
- 对于引用类型，== 判断两个变量是否引用同一个对象，而 equals() 判断引用的对象是否等价。

2. equals()`和`hashcode()的联系

- hashCode() 返回散列值，而 equals() 是用来判断两个对象是否等价。等价的两个对象散列值一定相同，但是散列值相同的两个对象不一定等价。在覆盖 equals() 方法时应当总是覆盖 hashCode() 方法，保证等价的两个对象散列值也相等。

1. String,StringBuilder,StringBuffer的区别

- 首先String,StringBuilder,StringBuffer都是基于字符数组实现的。String被设计为不可变类，当对String对象修改时会创建一个新对象。当对StringBuilder,StringBuffer对象修改时会修改字符数组的内容。

- String：适用于少量的字符串修改操作的情况

  StringBuilder：适用于单线程下在字符缓冲区进行大量操作的情况

  StringBuffer：适用多线程下在字符缓冲区进行大量操作的情况

2 集合框架

1. 讲一下JAVA中的集合

- java中的集合分为value(Collection),和key-value(Map)两种;存储value的有list和set两种: list是有序的,可重复的 set是无序的,不可重复的。存储为key-value是map:HashMap,Hashtable,CurrentHashMap

2. ARRAYLIST, Vector和LINKEDLIST的区别

- Vector、ArrayList都是以类似数组的形式存储在内存中，LinkedList则以链表的形式进行存储。
- Vector线程同步，ArrayList、LinkedList线程不同步。

3. HASHMAP和HASHTABLE的区别

2 面向对象

1. 面向对象特性？
2. 

3 并发



