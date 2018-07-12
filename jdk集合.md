
# 集合类(jdk1.8)

| 类   | 初始大小 |  自动扩容方式  | 元素是否允许null，重复及顺序性|最大容量 |是否线程安全 |优点/缺点|其他 |
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| ArrayList | 基于数组Object[] elementData,  new ArrayList()方式创建,在add第一个元素时自动扩容为容量10. 在add大量元素前，可以调用ensureCapacity调整容量已减少自动扩容次数(自动扩容可能会调用arrayCopy) | grow(minCapacity): newCapacity为Max( oldCapacity*2, minCapacity) | 允许null，可重复，按插入顺序 | Integer.MAX_VALUE | 非线程安全,转为线程全：List list = Collections.synchronizedList(new ArrayList())（创建时调用） | 常量时间：size,isEmpty,get,set,iterator,listIterator操作 O(n): add | public class ArrayList<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable |
| LinkedList | 基于双向列表: Node { E item,Node prev,Node next;} |  无 | 允许null，可重复,按插入顺序 | 理论上无限制 | 非线程安全 ,转为线程全：List list = Collections.synchronizedList(new LinkedList())（创建是调用）| 插入较快（不需要复制），但较耗空间 | public class LinkedList<E> extends AbstractSequentialList<E> implements List<E>, Deque<E>, Cloneable, java.io.Serializable |
| Vector | 基于数组Object[] elementData,new Vector() 方式创建初始大小为10，在add大量元素前，可以调用ensureCapacity调整容量已减少自动扩容次数(自动扩容可能会调用arrayCopy) | grow(minCapacity) ,capacityIncrement指定优先扩容数 | 允许null ，可重复，按插入顺序 | Integer.MAX_VALUE | 线程安全 ,无需考虑线程安全时使用ArrayList | 方法级别synchronized,线程安全但较ArrayList慢 | public class Vector<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable |
| Stack |  同Vector | 同Vector |  同Vector | 同Vector | 线程安全 | 同Vector,新增pop,push,peek,search操作 | public class Stack<E> extends Vector<E> |
| HashMap | 默认16，默认加载因子0.75 ，new HashTable()方式创建，在put时才分配容量16,容量总是2^*, transient Node<K,V>[] table | resize(),初始化为16或2*tableSize | key,value都允许null, 不重复，无序 | 2^30 | 非线程安全 ，转为线程安全：Collections.synchronizedMap(new HashMap()) |  | public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable |
| HashSet | 基于HashMap,默认16 |  | 允许null，不重复，无序 |  | 非线程安全,转为线程安全:Collections.synchronizedSet(new HashSet(...)); | 常量时间:add,remove,contains,size，重视循环遍历性能时(耗时: 元素数量+capacity)，capacity初始值不要设置太大或加载因子太小 | pblic class HashSet<E>  extends AbstractSet<E>  implements Set<E>, Cloneable, java.io.Serializable |
