## java 集合

![java集合框架的接口](https://s2.loli.net/2023/04/11/w4igrmzoEtGhqs6.png)

java 集合有两个基本接口：

- Collection
- Map

#### 数据结构：队列（queue）

队列（queue）这种数据结构的实现方式：

- 循环数组：比链表更高效，支持快速的随机访问，但它是一个有界集合，容量有限。
- 链表：不支持快速的随机访问，随机访问很慢。如果要查看链表中的第 n 个元素，就必
  须从头开始，越过 n-1 个元素，没有捷径可走。

#### 数据结构：映射（map）

知道某些关键信息，需要查找与之关联的元素，映射（map）数据结构就是为此设计：  
映射用来存放键值对，提供键，就能查到值。

### Collection 接口

#### 队列接口：Queue Deque 和 PriorityQueue

java 中 Queue （队列）接口特点：

- 可在尾部添加元素，头部删除元素
- 可查找队列中元素的个数
- 先进先出方式检索对象

java 中 Deque （双端队列）接口特点：

- 可在尾部和头部添加或删除元素
- 不支持在队列中间添加元素

java 中 PriorityQueue （优先队列）接口特点：

- 按照任意顺序插入，但会按照有序顺序检索

ArrayDeque 类的实现用的是循环数组队列，  
 LinkedList 类的实现使用的是链表，  
 它们都实现了 Deque 接口。

```java
// Collection 扩展了 Iterable 接口
public interface Collection<E> extends Iterable<E>{
  boolean add(E element);
  Iterator<E> iterator();
  .  .  .
  boolean isEmpty();
  boolean contails(Object obj);
  boolean contailsAll(Collection<?> c);
  .  .  .
}
// Iterable 接口
public interface Iterable<T> {
  Iterator<T> iterator();
    .  .  .
}
// 迭代器
public interface Iterator<E>{
  E next();
  boolean hasNext();
  void remove();
  default void forEachRemaining(Consumer<? super E> action)
}
```

java 集合类库中的迭代器不同于其他语言，传统集合类库迭代器是根据数组索引建模，可
以使用索引 i 直接找到元素 a[i]，java 迭代器查找一个元素的唯一方法是调用 next，可
以认为 java 迭代器位于两个元素之间，当调用 next 时，迭代器就越过下一个元素，并返
回刚刚越过的那个元素的引用。

#### Collection 接口实用方法

Collection 接口声明了很多有用的方法，所有的实现类都必须提供这些方法。例如：

- isEmpty()
- contains()
- containsAll()
- equals()
- addAll()
- remove()
