---
type: doc
layout: reference
category: "Syntax"
title: "异常"
---

# 异常(Exception)

## 异常类

Kotlin 中所有的异常类都是 `Throwable` 的后代类.
每个异常都带有一个错误消息, 调用堆栈, 以及可选的错误原因.

要抛出异常, 可以使用 *throw*{: .keyword } 表达式:

``` kotlin
throw MyException("Hi There!")
```

要捕获异常, 可以使用 *try*{: .keyword } 表达式:

``` kotlin
try {
    // 某些代码
}
catch (e: SomeException) {
    // 异常处理
}
finally {
    // 可选的 finally 代码段
}
```

*try*{: .keyword } 表达式中可以有 0 个或多个 *catch*{: .keyword } 代码段. *finally*{: .keyword } 代码段可以省略.
但是, *catch*{: .keyword } 或 *finally*{: .keyword } 代码段总计至少要出现一个.

### Try 是一个表达式

*try*{: .keyword } 是一个表达式, 也就是说, 它可以有返回值.

``` kotlin
val a: Int? = try { parseInt(input) } catch (e: NumberFormatException) { null }
```

*try*{: .keyword } 表达式的返回值, 要么是 *try*{: .keyword } 代码段内最后一个表达式的值, 要么是 *catch*{: .keyword } 代码段内最后一个表达式的值.
*finally*{: .keyword } 代码段的内容不会影响 *try*{: .keyword } 表达式的结果值.

## 受控异常(Checked Exception)

Kotlin 中不存在受控异常(checked exception). 原因有很多, 我们举一个简单的例子.

下面的例子是 JDK 中 `StringBuilder` 类所实现的一个接口:

``` java
Appendable append(CharSequence csq) throws IOException;
```

这个方法签名代表什么意思? 它说, 每次我想要将一个字符串追加到某个对象(比如, 一个 `StringBuilder`, 某种 log, 控制台, 等等), 我都必须要捕获 `IOException` 异常. 为什么? 因为这个对象有可能会执行 IO 操作 (比如 `Writer` 类也会实现 `Appendable` 接口)...
因此就导致我们的程序中充满了这样的代码:

``` kotlin
try {
    log.append(message)
}
catch (IOException e) {
    // 实际上前面的代码必然是安全的
}
```

这样的结果就很不好, 参见 [Effective Java](http://www.oracle.com/technetwork/java/effectivejava-136174.html), 第 65 条: *不要忽略异常*.

Bruce Eckel 在 [Java 需要受控异常吗?](http://www.mindview.net/Etc/Discussions/CheckedExceptions) 一文中说:

> 在小程序中的试验证明, 在方法定义中要求标明异常信息, 可以提高开发者的生产性, 同时提高代码质量, 但在大型软件中的经验则却指向一个不同的结论 – 生产性降低, 而代码质量改善不大, 或者根本没有改善.

另外还有其他一些这类讨论文章:

* [Java 的受控异常是一个错误](http://radio-weblogs.com/0122027/stories/2003/04/01/JavasCheckedExceptionsWereAMistake.html) (Rod Waldhoff)
* [受控异常带来的问题](http://www.artima.com/intv/handcuffs.html) (Anders Hejlsberg)(译注, Borland Turbo Pascal 和 Delphi 的主要作者, 微软.Net概念的发起人之一, .Net首席架构师)

## 与 Java 的互操作性

关于与 Java 的互操作性问题, 请参见 [与 Java 的互操作性](java-interop.html) 中关于异常的小节.
