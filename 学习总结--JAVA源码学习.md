## String

### 考点

- 为什么 String 类型要用 final 修饰？

  > 安全、高效

- == 和 equals 的区别是什么？

  >== 比较引用地址，equals用于比较值

- String 和 StringBuilder、StringBuffer 有什么区别？

  > 拼接效率：StringBuider>StringBuffer(线程安全)>String

- String 的 intern() 方法有什么含义？

  > JDK1.8之后，字符串常量池移到堆内存中，new String("xxx")直接在堆上创建对象，调用intern()方法将此字符串保存到常量池

- String 类型在 JVM（Java 虚拟机）中是如何存储的？编译器对 String 做了哪些优化？