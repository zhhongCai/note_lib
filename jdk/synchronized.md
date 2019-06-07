1. 相当于对调用方法add的实例加锁， 同实例只允许一个线程执行add，两个线程可以同时调用不同A的实例的add方法
public class A {
private int count;
public synchronized void add(int value) {
    this.count += value
}
}

等价于
public void add(int value) {
  synchronized(this) {
    this.count += value
    }
}

2. 相当于对A类的所有实例加锁，所有A的实例，只允许一个线程在执行add.
public class A {
private int count;
public static synchronized void add(int value) {
    this.count += value
}
}
等价于
public void add(int value) {
  synchronized(A.class) {
    this.count += value
    }
}

//TODO　待续...
