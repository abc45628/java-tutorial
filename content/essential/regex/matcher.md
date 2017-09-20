# Matcher 的方法

本节介绍了Matcher类的一些其他有用的方法。为了方便起见，下列方法根据功能进行分组。

## 索引方法

索引方法提供了有用的索引值，它们精确地显示了输入字符串中匹配的位置：

* `public int start()`：返回上一个匹配的起始索引。
* `public int start(int group)`：返回上次匹配操作期间给定组捕获的子序列的起始索引。
* `public int end()`：返回最后一个字符匹配后的偏移量。
* `public int end(int group)`：返回在上一次匹配操作期间由给定组捕获的子序列的最后一个字符之后的偏移量。

## 检查方法

* `public boolean lookingAt()`：尝试将输入序列从区域开头开始与模式相匹配。
* `public boolean find()`：尝试找到匹配模式的输入序列的下一个子序列。
* `public boolean find(int start)`：重置此匹配器，然后尝试从指定的索引开始找到与模式匹配的输入序列的下一个子序列。
* `public boolean matches()`：尝试将整个区域与模式进行匹配。

## 替换方法

替换方法是替换输入字符串中的文本的有用方法。.

* `public Matcher appendReplacement(StringBuffer sb, String replacement)`：执行非终端附加和替换步骤。
* `public StringBuffer appendTail(StringBuffer sb)`：实现终端附加和替换步骤。
* `public String replaceAll(String replacement)`：将与模式匹配的输入序列的每个子序列替换为给定的替换字符串。
* `public String replaceFirst(String replacement)`：将与模式匹配的输入序列的第一个子序列替换为给定的替换字符串。
* `public static String quoteReplacement(String s)`：返回String指定的文字替换String。该方法产生一个在类的方法中String作为文字替换s的appendReplacement方法Matcher。所String产生的字符串s将作为字面序列处理。斜杠（'\'）和美元符号（'$'）将没有特殊意义。

## 使用 start 和 end 方法
下面的例子中将计算输入字符串中出现“dog”一词的次数。

```java
String REGEX = "\\bdog\\b";
String INPUT = "dog dog dog doggie dogg";
Pattern p = Pattern.compile(REGEX);
Matcher m = p.matcher(INPUT);
int count = 0;
while (m.find()) {
    count++;
    System.out.println("匹配个数：" + count +
                               " start()：" + m.start() +
                               " end()：" + m.end()
    );
}

---- Output ----
匹配个数：1 start()：0 end()：3
匹配个数：2 start()：4 end()：7
匹配个数：3 start()：8 end()：11

```

您可以看到，此示例使用字边界来确保字母"d" "o" "g"不仅仅是长字词中的子字符串。在输入字符串中发生匹配的地方,它还提供了一些有用的信息。该start方法返回给定组在先前匹配操作中捕获的子序列的开始索引，并end返回匹配的最后一个字符的索引。

# 使用matches和lookingAt方法

matches和lookingAt方法都尝试将输入序列与模式进行匹配。然而，差异在于，matches要求整个输入序列匹配，lookingAt而不需要。两种方法始终从输入字符串的开头开始。下面是示例

```java
String REGEX = "foo";
String INPUT = "fooooooooooooooooo";
Pattern p = Pattern.compile(REGEX);
Matcher m = p.matcher(INPUT);

System.out.println("lookingAt():" + m.lookingAt());
System.out.println("matches():" + m.matches());

---- Output ----
lookingAt():true   // 仅当输入序列的前缀匹配此匹配器的模式时才返回 true。
matches():false
```

## 使用 replaceFirst(String) and replaceAll(String)

在replaceFirst与replaceAll方法替换给定的正则表达式匹配的文本。按照他们的名字显示，replaceFirst替换第一次出现，并replaceAll替换所有的事件。

```java
 String REGEX = "dog";
    String INPUT = "The dog says meow. All dogs say meow.";
    String REPLACE = "cat";
    Pattern p = Pattern.compile(REGEX);
    Matcher m = p.matcher(INPUT);

    System.out.println("replaceAll():" + m.replaceAll(REPLACE));
    System.out.println("replaceFirst():" + m.replaceFirst(REPLACE));
    
    
---- Output ----
replaceAll():The cat says meow. All cats say meow.
replaceFirst():The cat says meow. All dogs say meow.
```

在第一个版本中，所有出现的代码dog都被替换cat。但为什么要停在这里？而不是替换一个简单的文字dog，您可以替换匹配任何正则表达式的文本。该方法的API指出，“给定正则表达式`a*b`，输入`aabfooaabfooabfoob`和替换字符串`-`，在该表达式的匹配器上调用此方法将产生字符串`-foo-foo-foo-`。

```java
 String REGEX = "a*b";
    String INPUT = "aabfooaabfooabfoob";
    String REPLACE = "-";
    Pattern p = Pattern.compile(REGEX);
    Matcher m = p.matcher(INPUT);

    System.out.println("replaceAll():" + m.replaceAll(REPLACE));
    System.out.println("replaceFirst():" + m.replaceFirst(REPLACE));
    
---- Output ----
replaceAll():-foo-foo-foo-
replaceFirst():-fooaabfooabfoob
```
巩固下量词的知识：`a*b` 表示ab或则b 都符合条件，但是是贪婪量词，会出现长度零匹配的结果。所以就出现了上面的输出效果


## 使用`appendReplacement(StringBuffer,String) and appendTail(StringBuffer)`

Matcher类还提供appendReplacement和appendTail文本替换方法。使用这两种方法来实现与之相同效果的 replaceAll

```java
String REGEX = "a*b";
String INPUT = "aabfooaabfooabfoob";
String REPLACE = "-";
Pattern p = Pattern.compile(REGEX);
Matcher m = p.matcher(INPUT);

StringBuffer sb = new StringBuffer();
while(m.find()){
    m.appendReplacement(sb,REPLACE);
}
m.appendTail(sb);
System.out.println(sb.toString());

---- Output ----
-foo-foo-foo-
```

## 与java.lang.String 等效的方法
为了方便起见，String该类也模仿了几种Matcher方法：

* `public String replaceFirst(String regex, String replacement)`：用给定的替换替换与给定正则表达式匹配的此字符串的第一个子字符串。这种形式的这种方法的调用产生与表达式完全相同的结果str.replaceFirst(regex, repl)Pattern.compile(regex).matcher(str).replaceFirst(repl)

* `public String replaceAll(String regex, String replacement)`：用给定的替换替换与给定正则表达式匹配的此字符串的每个子字符串。这种形式的这种方法的调用产生与表达式完全相同的结果str.replaceAll(regex, repl)Pattern.compile(regex).matcher(str).replaceAll(repl)
