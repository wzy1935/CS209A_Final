# 参考答案

我自己做的，真的只能做参考...不是标准答案，不确定的话还是查一下。

## I

**1-5** ADCBC

**6-10** BDABA

**11-15** CCCAA



## II

**1**

耦合度是指一个类对外的关联程度，如果关联了很多的类，耦合度就高，反之耦合度就低；聚合度是指一个类内部承担职责之间的相关程度，职责比较接近，内聚度就高。软件设计目标是：低耦合，高内聚。

**2**

Java的泛型检查，发生在编译期。类型擦除后泛型参数会被替换成 Object 。原始类型不是 Object，自然不能在泛型中使用。

**3**

`RetentionPolicy.RUNTIME`。Autowired 使用了依赖注入。Spring 在初始化时会初始化各个组件对象。遇到被 `@Autowired` 修饰的域或方法，即需要被注入的对象时，会按照类型去容器中找到对应的组件通过反射进行装配。

**4**

Thread 的 run 方法会直接调用我们传入的 Runnable 的 run 方法，也就是说，这只是一次普通的方法调用，并不会开启新线程。start方法才会开启新线程。



## III

**1**

```
a=300, b=1, s=hello
```

**2**

```
5
```

**3**

```
5
9
```

**4**

```
C:\User\a\b\..\c\.\..\d.md
C:\User\a\d.md
```



## IV

**1**

输出260。这是因为equals方法没有被正确重写，导致在比较是否相同时调用了父类的equals，而默认equals比较的是引用地址，所以导致HashSet认为所有对象都不相同。

可以对 equals 方法做以下修改：

```java
@Override
public boolean equals(Object o) {
    if (o instanceof CharPair) {
        return ((CharPair) o).x == x && ((CharPair) o).y == y;
    } else {
        return false;
    }
}
```

**2** 

```java
Stream.generate(Math::random)
        .limit(50)
        .forEach(System.out::println);
```

**3**

```java
public class MyBlockingQueue<E> {
    private Queue<E> queue = new LinkedList<>();
    Lock lock = new ReentrantLock();
    Condition notEmpty = lock.newCondition();
    Condition notFull = lock.newCondition();
    private int size;

    public MyBlockingQueue(int size) {
        this.size = size;
    }

    public void put(E element) throws InterruptedException {
        try {
            lock.lock();
            while (queue.size() >= size) {
                notFull.await();
            }
            queue.add(element);
            notEmpty.signal();
        } finally {
            lock.unlock();
        }
    }

    public E take() throws InterruptedException {
        try {
            lock.lock();
            while (queue.size() == 0) {
                notEmpty.await();
            }
            E output = queue.remove();
            notFull.signal();
            return output;
        } finally {
            lock.unlock();
        }
    }
}
```