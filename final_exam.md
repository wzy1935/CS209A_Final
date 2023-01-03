# 期末考题

## I 单选

**1、** final修饰符的作用是社么？

A 表示变量无法被修改

B 表示变量只能被类拥有而不是被实例拥有

C 使变量不参与对象序列化

D 控制线程同步



**2、 ** 下面哪一个不是 `FilterInputStream` 的子类？

A DigestInputStream

B DataInputStream

C BufferedInputStream

D FileInputStream



**3、** 下面哪一个没有继承 `Collection<E>` ？

A Set

B List

C Map

D Queue



**4、** 下面哪个不支持泛型？

A List

B Array

C Map

D Set



**5、** 所有 `Collection` 都实现了哪个接口？

A Comparable

B Cloneable

C Iterable

D Serializable



**6、** 下面关于序列化对象的描述，错误的是：

A 对象可以序列化到数据库中

B 对象不可以序列化到文件中

C 对象可以被序列化为流

D 对象可以通过序列化进行网络传输



**7、** Java函数式编程中，下面的哪个方法不接受参数，但有返回值？

A Consumer 中的 accept 函数

B Predicate 中的 test 函数

C Function 中的 apply 函数

D Supplier 中的 get 函数



**8、** 不同线程会共享如下哪一部分的内存？

A Heap Area

B Stack Area

C Native Method Stack

D PC Register



**9、 ** 服务器端的 `ServerSocket` 使用哪个方法来接收客户端的连接？

A bind

B accept

C listen

D wait



**10、** `ArrayList` 中多线程使用迭代器时的错误机制是：

A fail-fast

B fail-safe

C fail-quit

D fail-stop



**11、** Java EE 从概念上来看更接近于是一个：

A implementation

B container

C architecture

D pattern



**12、** 下面哪个具有 `doGet`、 `doPost` 、`doDelete` 方法？

A Servlet

B GenericServlet

C HttpServlet

D 以上都有



**13、** Spring框架采用的默认web服务器是：

A Spring MVC

B Thymeleaf

C Apache Tomcat

D Docker



**14、** JavaFX中，窗体内的所有可视组件节点（控件、布局等）都附着在什么上？

A Scene

B Stage

C Control

D Scene Graph



**15、** 运行main方法的线程名为：

A main

B main-0

C Thread-0

D Thread-main



## II 简答

**1、** Cohesion 和 Coupling 指的是什么？软件设计中，与之对应的原则是什么？



**2、** 为什么在使用泛型时不能使用原始类型（Primitive Type）而必须使用引用数据类型（Reference Type）？



**3、** Spring 中的 `@Autowried` 注解的保留注解（Retention Policy）是什么？ `@Autowried` 的原理是什么？



**4、** Thread 本身就实现了 Runnable 的 run 方法，那为什么在多线程情境下，我们要调用 start 方法而不是 run 方法？



## III 程序分析

**1、** 考虑以下 Junit 测试：

```java
public class MyTest {
    static int a = 100;
    String s = "hi";
    int b = 0;

    @BeforeAll
    public static void startUp() {
        a = 200;
    }

    @BeforeEach
    public void init() {
        s = "hello";
        b = 1;
    }

    @Test
    public void test1() {
        a = 300;
        b++;
        s = "bye";
    }

    @Test
    public void test2() {
        // what is the output?
        System.out.printf("a=%d, b=%d, s=%s", a, b, s);
    }

    @AfterEach
    public void terminate() {
        b *= 3;
    }

    @AfterAll
    public static void cleanUp() {}
}
```

如果 test1 在 test2 前运行，试给出程序的输出。



**2、** 写出下面程序的输出：

```java
Class<?> clazz = Class.forName("java.lang.String");
Method m = clazz.getMethod("indexOf", String.class, int.class);
String str = "This is a sentence in the program.";
System.out.println(m.invoke(str, "i", 3));
```



**3、** 考虑以下类和接口：

CharCount.java

```java
public interface CharCount {
    int count(String s);
}
```

StringContainer.java

```java
public class StringContainer {
    String prefix;

    public StringContainer(String prefix) {
        this.prefix = prefix;
    }

    public static int length(String str) {
        return str.length();
    }

    public int combinedLength(String str) {
        return prefix.length() + str.length();
    }
}
```

给出下面程序的输出：

```java
StringContainer container = new StringContainer("test");
CharCount c1 = StringContainer::length;
CharCount c2 = container::combinedLength;
System.out.println(c1.count("hello"));
System.out.println(c2.count("hello"));
```



**4、** 假设当前路径为 `C:\User` ，给出下面程序的输出：

```java
Path path = Path.of("a/b/../c/./../d.md");
System.out.println(path.toAbsolutePath());
System.out.println(path.normalize().toAbsolutePath());
```



## IV 编程

**1、** 小明编写了一个 `CharPair` 类。为了在 `HashSet` 中正常使用，他重写了 hashCode 和 equals 方法。他在还main方法中测试了这个类。按小明的预期，测试应该输出26。但实际的输出结果却不同。

```java
public class CharPair {
    char x;
    char y;

    public CharPair(char x, char y) {
        this.x = x;
        this.y = y;
    }

    public int hashCode() {
        return 31 * x + y;
    }

    public boolean equals(CharPair charPair) {
        return x == charPair.x && y == charPair.y;
    }

    public static void main(String[] args) {
        Set<CharPair> pairs = new HashSet<>();
        for (int i = 0; i < 10; i++) {
            for (char c = 'a'; c <= 'z'; c++) {
                pairs.add(new CharPair(c, c));
            }
        }
        System.out.println(pairs.size());
    }
}
```

- 给出实际的输出结果，并分析输出不是26的原因；
- 修改上述代码，使其能正常输出26（你只需要修改其中一个方法的代码）。



**2、** 编写一段程序，使其能生成无限个浮点数（double），并输出前50个数。你必须使用Stream API。使用除了Stream以外的做法不得分（提示：`Math.random()`）。



**3、** 生产者-消费者问题定义如下：一组生产者进程和一组消费者进程共享一个初始为空、大小为n的缓冲区，只有缓冲区为满时，生产者才把消息放入缓冲区，否则必须等待；只有缓冲区不为空时，消费者从中取出消息，否则必须等待。由于缓冲区是临界资源，它只允许一个生产者放入消息，或一个消费者从中取出消息。

Java中，可以使用 `BlockingQueue` 来解决生产者-消费者问题。 `BlockingQueue` 有两个主要方法：`put` 和 `take` 。 `BlockingQueue` 有一个固定的容量。当生产者线程调用 `put` 放入元素时，如果队列已满，就进行等待，直到队列有空余空间；当消费者线程调用 `take` 取出元素时，如果队列为空，就进行等待，直到队列有新元素进入。

类似的，在本题中，你需要在下面的模板的基础上实现一个 `MyBlockingQueue<E>` 类，其也有 `put` 和 `take` 方法。它要能够正确处理多线程下的访问，正确的线程同步顺序，以及消费者和生产者的实时通知。你可以使用 `ReentrantLock` 和 `Condition` 来进行线程间的同步和通信。

```java
public class MyBlockingQueue<E> {
    private Queue<E> queue;
    private int size;

    public MyBlockingQueue(int size) {

    }

    public void put(E element) {

    }

    public E take() {

    }
}
```

