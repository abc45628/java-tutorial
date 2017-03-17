# Collection 接口
Collection 表示一组对象，这些对象也称为 collection 的元素。按照惯例，所有通用的 Collection 实现类（通常通过它的一个子接口间接实现 Collection）都有一个接受 Collection 参数的构造函数。此构造函数称为`转换构造函数`，初始化新集合已包含指定集合中的所有元素，而不管给定集合的子接口或实现类。换句话说，它允许你转换集合的类型。

比如：假设你有一个Collection<String> c,可是一个List或则一个Set，或另一种Collection。这样你就正转换：
```
List <String> list = new ArrayList <String>（c）;
```

Collection接口包含执行基本操作，如:

```java
int size()，boolean isEmpty()， boolean contains(Object element)，
boolean add(E element)，boolean remove(Object element)，和 Iterator<E> iterator()。
```

它还包含操作整个集合的方法,如:
```java
boolean containsAll(Collection<?> c)， boolean addAll(Collection<? extends E> c)， 
boolean removeAll(Collection<?> c)，boolean retainAll(Collection<?> c)，和 void clear()。
```
额外的数组操作方法，如:
```java
Object[] toArray()和<T> T[] toArray(T[] a)
```

在JDK 8和更高版本中，Collection接口还公开了方法Stream<E> stream()，并Stream<E> parallelStream()从底层集合获取顺序或并行流。（有关使用流的详细信息，请参阅标题为“ [聚合操作](http://docs.oracle.com/javase/tutorial/collections/streams/index.html) ”的课程 。）


Collection 表示一组对象，它有方法告诉你集合（size，isEmpty）中有多少元素，检查给定对象是否在collection（contains）中的方法，从集合（add，remove）中添加和删除元素的方法，以及提供迭代器对collection（iterator）；

**如add:**如果此 collection 由于调用而发生更改，则返回 true。（如果此 collection 不允许有重复元素，并且已经包含了指定的元素，则返回 false。）

**如remove**：如果此 collection 包含指定的元素（或者此 collection 由于调用而发生更改），则返回 true 。 


## 遍历集合
有三种遍历方式

1. stream ( 1.8 +)
2. forEach
3. Iterators

stream是1.8的，不在1.7范围了，就不说了。forEach也不说了。

### 迭代器
Iterator是一个对象，使您能够遍历集合，并根据需要从集合中有选择性地删除元素。Iterator通过调用它的iterator方法获得一个集合。以下是Iterator接口。


```java
public interface Iterator <E> { 
    boolean hasNext（）; 
    E next（）; 
    void remove（）; // optional 
}
```

**注意**，这Iterator.remove是在迭代期间修改集合的唯一安全方法;在迭代期间使用其他修改方式应该都会报错？

当你需要以下操作时，应该选使用Iterator而不是for-each：
- 迭代中删除元素，forEach 也是使用的Iterator，但是语法糖上隐藏了Iterator的相关操作
- 遍历多个并行集合（！没有明白是什么意思）

以下方法使用Iterator来过滤任意Collection，删除特定元素：
···java
    static void filter(Collection<?> c) {
        for (Iterator<?> it = c.iterator(); it.hasNext(); )
            // 唯一不同的就是这里 条件的判断
            if (!cond(it.next())) {
                it.remove();
            }
    }
```
这段代码是多态的，意味着使用与任何 Collection 实现。此示例演示使用Java集合框架编写多态性算法是多么容易。


## 集合接口批量操作
批量操作对整个集合进行操作。你可以使用的基本操作实现这些简化操作,尽管在大多数情况下这样的实现将是低效率的。

- containsAll - 如果此 collection 包含指定 collection 中的所有元素，则返回 true。
- addAll -  将指定 collection 中的所有元素都添加到此 collection 中（可选操作）。
- removeAll -  移除此 collection 中那些也包含在指定 collection 中的所有元素（可选操作）。
- retainAll- 从目标中删除Collection所有不包含在指定中的元素Collection。也就是说，它只保留目标Collection中那些也包含在指定的元素Collection。
- clear - 移除此 collection 中的所有元素（可选操作）。

如果目标Collection被修改，addAll，removeAll以及retainAll方法都返回true，

简单示例
```java
        ArrayList<String> c = new ArrayList<>();
        c.add("h");
        c.add(null);
        ArrayList<String> e = new ArrayList<>();
        e.add("h");
        e.add("l");
        c.removeAll(Collections.singletonList("h"));
        System.out.println(c); //[null]
        // 删除集合中所有为null的值
        c.removeAll(Collections.singleton(null));
        System.out.println(c); //[]
        
```
`Collections.singletonList`要注意使用，他是返回只有一个元素的集合，所以不要这样使用`Collections.singletonList(e)`.(除非你匹配的list中元素也是一个 e 类型的list)


## 集合接口数组操作

`toArray`此方法充当基于数组的 API 与基于 collection 的 API 之间的桥梁。
```java
        ArrayList<String> e = new ArrayList<>();
        e.add("h");
        e.add("l");
        // 该方法因为不确定是什么类型，就返回Obejct
        Object[] objects = e.toArray();
        System.out.println(objects);
        // 如果你明确的知道 e 是一个String泛型集合，那么可以把这些数据拷贝到这个新的数组中。
        String[] array = e.toArray(new String[e.size()]);
        System.out.println(array);
```