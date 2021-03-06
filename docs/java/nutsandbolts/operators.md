# 运算符

现在你已经学会了如何声明和初始化变量，你可能想知道如何与他们做某事。学习 Java 编程语言的运算符是开始的好地方。运算符是对一个，两个或三个操作数执行特定操作的特殊符号，然后返回一个结果。

当我们探索 Java 编程语言的运算符时，可能有助于您提前知道哪些运算符具有最高优先级。下表中的运算符按照优先顺序列出。出现的表格越靠近，它的优先级越高。具有较高优先级的运算符在具有较低优先级的运算符之前进行评估。同一行的运算符具有同等的优先权。当相同优先级的运算符出现在同一表达式中时，规则必须对首先进行评估的规则进行管理。除赋值运算符之外的所有二进制运算符**从左到右进行评估**; 分配运算符从左到右进行评估。

运算符优先级

| 运算符    | 优先级                                     
|-----------|-----------------------------------------
| 后缀      | `expr+=`  `expr--`                        
| 一元      | `++expr` `--expr` `+expr` `-expr` `~` `!`
| 乘除      | `*` `/` `%`                              
| 加减      | `+` `-`                                  
| 转移      | `<<` `>>` `>>>`                          
| 相关的    | `<` `>` `<=` `>=` `instanceof`            
| 平等      | `===` `!=`                               
| 按位 AND  | `&`                                      
| 按位 异或 | `^`                                        
| 按位 OR   | `|`                                       
| 逻辑与    | `&&`                                       
| 逻辑或    | `||`                                        
| 三元      | `?` `:`                                    
| 分配      | `=` `+=` `-=` `*=` `/=` `%=` `&=` `^=` `\`  `=` `<<=` `>>=` `>>>=`

在通用编程中，某些运算符往往比其他运算符更频繁出现; 例如，赋值运算符「=」比无符号右移运算符「>>>」 更常见。考虑到这一点，以下讨论首先关注您最有可能重点使用的运算符，并定期关注那些较不常见的运算符。每个讨论都附有可以编译和运行的示例代码。研究其产出将有助于巩固您刚学到的知识。
