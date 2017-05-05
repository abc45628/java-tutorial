# 线程干扰
考虑一个简单的类
```java
class Counter {
    private int c = 0;

    public void increment() {
        c++;
    }

    public void decrement() {
        c--;
    }

    public int value() {
        return c;
    }

}
```
Counter被设计成使得每次调用`increment`将添加1到c，并且每次调用`decrement`将从c-1。然而，如果一个`Counter`对象被多个线程引用，则线程之间的干扰可能会阻止这样的预期。

干扰发生在两个操作，运行在不同的线程，但作用与相同的数据。而且是交错调用。这以为者这两个操作由多个步骤组成，并且步骤序列重叠。

Counter由于这两个操作都是单一的简单语句，因此可能不会对实例进行交错操作。然而，即使简单的语句也可以转换为虚拟机的多个步骤。我们不会检查虚拟机所采取的具体步骤，只要知道单个表达式 c++ 可以分解成以下三个步骤：

1. 检索当前值c
2. 将检索到的值增加1
3. 将增加的值存储回来c

表达式 c-- 可以以相同的方式分解，除了第二步是减少而不是增加

假设线程A 调用 `increment`的同一时间，线程B调用`decrement`.如果初始值 c = 0.他们的交错操作可能遵循以下顺序：

1. 线程A：检索c
2. 线程B：检索c
3. 线程A：增加检索值；结果是1
4. 线程B：减去检索值；结果是-1
5. 线程A：将结果存储在c中，c现在是1
6. 线程B：将结果存储在c中，c现在是-1

线程A的结构丢失，被线程B覆盖了。这种特定的交错只是一种可能性。在不同的情况下，线程B的结果可能会丢失，或则完全没有错误。因为他们是不可预测的，线程干扰的bug可能难以检测和修复。