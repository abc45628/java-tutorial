# 原子访问

在编程中，原子动作是一次有效地发生的。原子行动不能在中间停止：它完全发生，或者根本就不会发生。在动作完成之前，不会看到原子动作的副作用。

我们已经看到增量表达式，如 c++，不描述原子动作。即使非常简单的表达式也可以定义可以分解为其他操作的复杂动作。但是，您可以指定的是原子的操作：

* 读取和写入对于引用变量和大多数原始变量（除了 long 和 double）之外的所有类型都是原子的。
* 读取和写入对于 volatile 声明的所有变量 （包括 long 和 double 变量）都是原子的。

原子动作不能交错，所以可以使用它们，而不用担心线程干扰。但是，这并不能消除所有需要同步原子动作的原因，
因为内存一致性错误仍然是可能的。使用 `volatile` 变量降低了内存一致性错误的风险，
因为对 `volatile` 变量的任何写入都会建立与该同一个变量的后续读取相关的发生关系。
这意味着 `volatile` 变量对其他线程总是可见的。此外，这也意味着当一个线程读取一个 `volatile` 变量时，
它不仅仅看到最新的变化 `volatile`，而且还会看到导致变化的代码的副作用。

使用简单的原子变量访问比通过同步代码访问这些变量更有效，但是程序员需要更多的谨慎以避免内存一致性错误。
额外的努力是否值得取决于应用程序的大小和复杂性。

java.util.concurrent 包中的某些类 提供了不依赖于同步的原子方法。我们将在 [高级并发对象部分](./highlevel.html) 中讨论它们。
