### 抽象类与接口

#### 抽象类

- 可以包含普通类的任何特征（比如具体的属性和方法）
- 包含抽象的方法（必须为非 private/static）
- 不能实例化

#### 接口

接口不是类，它指特定功能的集合

```java
public abstract class bird {
    public abstract void 开始烹饪();
    public abstract void 出锅啦();
}
```
