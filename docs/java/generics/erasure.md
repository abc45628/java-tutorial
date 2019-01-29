# 类型擦除

泛型被引入到Java语言中以在编译时提供更严格的类型检查，并支持泛型编程。为了实现泛型，Java编译器将类型擦除应用于：

* 如果类型参数是无界的，则将泛型类型中的所有类型参数替换为它们的边界或对象。所产生的字节码因此只包含普通的类，接口和方法。

* 如果需要，插入类型可以保持类型安全。
* 生成桥接方法以保留扩展泛型类型中的多态性。

类型擦除确保没有为参数化类型创建新的类; 因此，泛型不会导致运行时开销。