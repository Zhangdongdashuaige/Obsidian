## Map及其实现类对比：
Java.util.Map:存储一对一对的数据（key-value键值对）
-  **HashMap**:主要实现类；线程不安全的，效率高；可以添加null的key和value值；底层使用**数组+单向链表+红黑树结构**存储。
	- **LinkedhashMap**:是HashMap的子类；在HashMap使用的数据结构的基础上，增加了一对双向链表，用于记录元素的先后顺序。（开发中，如果需要进行频繁的遍历操作，则推荐使用此类）
- **TreeMap**：底层使用**红黑树**存储；可以按照添加的key-value中的key元素的指定的属性的大小顺序进行遍历。需要考虑使用 ① 自然排序 ② 定制排序。
- **Hashtable**：古老实现类；线程安全的，效率低；不可以添加null的key或value值；底层使用**数组+单向链表**存储。
	- **Properties**：其key和value都是String类型。
``` java

```
