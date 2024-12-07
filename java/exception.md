### 异常

1. 检查型异常：也叫受检异常，编译器会检查你是否知道这个异常并做好了准备来处理该异常。
2. 非检查型异常：也叫非受检异常，编译器并不期望你为这些异常提供处理器，而是应该极力避免这种异常的出现。

### 集中异常处理策略

1. 如果一个方法包含一条可能抛出检查型异常的语句，可以直接在方法名上增加一个 throws 语句。

```java
public static void doSomethingWithClass(String name) throws ReflectiveOperationException{
  Class cl = Class.forName(name);// 可能抛出异常；
  // do something
}
```
