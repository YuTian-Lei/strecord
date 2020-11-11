### 迭代器操作集合
#### Iterator遍历
- 不能操作集合的增加和删除操作（改变了modCount）
- 可以迭代器的remove操作（同步了expectedModCount）
- 每次next()之前会比较modCount和expectedModCount

#### Foreach遍历
- 本质上来讲，Foreach其实就是在使用迭代器
- 本能进行集合操作（增加和删除）

### &和&&，|和||的区别
- |和&，前后都会执行
- ||和&&,前置条件满足就不再执行后序条件

### ArrayList扩容
- 进行add时，首次分配长度为10的数组

### 容器隐形迭代
- 进行hashcode和equals方法（遍历，如Indexof）
- 当容器作为另一个容器的元素时
- containAll、removeAll和retainAll方法
- 容器作为参数的构造函数

### Collections.synchronizedxxx
- Collections.synchronizedCollection(Collection<T>t)
- Collections.synchronizedList(List<T>list)
- Collections.synchronizedMap(Map<K, V>map)
- Collections.synchronizedSet(Set<T> t)

### 中断

public static boolean interrupted()| 测试当前线程是否已经中断。线程的中断状态 由该方法清除。换句话说，如果连续两次调用该方法，则第二次调用将返回 false（在第一次调用已清除了其中断状态之后，且第二次调用检验完中断状态前，当前线程再次中断的情况除外）
-|-
public boolean isInterrupted() |测试线程是否已经中断。线程的中断状态不受该方法的影响
public void interrupt()|中断线程