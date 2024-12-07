## lambda 表达式

### lambda 表达式的产生来源

```java
// 定制一个比较器根据字符串的长度来排序
class LengthComparator implements Comparator<String> {
    @Override
    public int compare(String o1, String o2) {
        return o1.length() - o2.length();
    }
}
var strings = new String[]{"343rere", "abc", "cdfefef", "a", "adeeefffeerf"};
// Arrays.sort(strings); 默认排序
Arrays.sort(strings, new LengthComparator());
```

如上所示，我们只是想实现一个自定义的排序方法，但不得不为了它单独定义一个类，并实
现自定义的方法，在使用的时候传递这个类的一个实例。有没有像其他语言那样可以直接传
递一个函数而不用创建对象呢？

我们可以像下面这样构建一个匿名类来简化

```java
var strings = new String[]{"343rere", "abc", "cdfefef", "a", "adeeefffeerf"};
Arrays.sort(strings, new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
        return o1.length() - o2.length();
    }
});
```

进一步简化成如下这样：

```java
var strings = new String[]{"343rere", "abc", "cdfefef", "a", "adeeefffeerf"};
Arrays.sort(strings, (String o1,String o2) -> o1.length() - o2.length());
```

`(String o1,String o2) -> o1.length() - o2.length());`像这种带参数变量的表达式就
叫做 lambda 表达式。  
编译器可以自动推导类型，还可以再进一步简化成如下这样：

```java
var strings = new String[]{"343rere", "abc", "cdfefef", "a", "adeeefffeerf"};
Arrays.sort(strings, (o1, o2) -> o1.length() - o2.length());
```

还可以更进一步简化为方法引用：

```java
var strings = new String[]{"343rere", "abc", "cdfefef", "a", "adeeefffeerf"};
Arrays.sort(strings, Comparator.comparingInt(String::length));
```

### 函数式接口

只有一个抽象方法的接口(可以有别的方法，但抽象方法只能有一个)称为函数式接口。  
上例中的 Comparator 就是一个函数式接口，我们用 lambda 表达式实现了它的 compare
抽象方法。最好把 lambda 表达式看作是一个函数，而不是一个对象，另外要接受 lambda
表达式可以传递到函数式接口，java 并没有为语言增加函数类型，其实像 lambda 表达式
所能做的也只是转换为函数式接口。

java api 在 java.util.function 包中定义了很多非常通用的函数式接口。

### 方法引用

```java
import javax.swing.Timer;
// ······

public class Main {
    public static void main(String[] args) {
        // 1 构造类的使用方式太麻烦就不写了
        // 2 普通使用方式
        var timer = new Timer(1000, new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println(e);
            }
        });
        // 3 使用lambda表达式
        var timer1 = new Timer(1000, e -> System.out.println(e));
        // 4 使用方法引用
        var timer2 = new Timer(1000, System.out::println);
    }
}
```

`System.out::println`就是一个方法引用，它指示编译器生成一个函数式接口的实例，覆
盖这个接口的抽象方法来调用给定的方法，本质就是上例中第二种方式。

### 构造器引用

```java
public class Main {
    public static class Person {
        private String name;
        private int age;

        public Person(String name) {
            this.name = name;
        }

        @Override
        public String toString() {
            return "Person{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    '}';
        }
    }

    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList(Arrays.asList("樊晓超", "宋晓青", "梵穆圣"));
        Stream<Person> stream = names.stream().map(Person::new);
        Person[] people = stream.toArray(Person[]::new);
        System.out.println(Arrays.toString(people));
    }
}
```

### lambda 表达式的变量作用域

lambda 分三个部分：

1. 参数
2. 代码块
3. 自由变量的值，指非参数而且不在代码块中定义的变量

这个自由变量有一个限制：必须是不会改变的变量，也就是说必须是一个事实最终变量。  
lambda 表达式中使用 this 时，是指创建这个 lambda 表达式的方法的 this，而不是这个
lambda 表达式。  
lambda 表达式就是闭包。

```java
public static void countDown(int start, int delay) {
    ActionListener listener = event -> {
        System.out.println(start);
    };
    new Timer(delay, listener).start();
}
```
