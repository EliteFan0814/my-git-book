## maven 包管理工具

### JVM 的工作流程

1. 执行一个类的字节码
2. 执行过程中碰到新的类，搜索新的类，并加载它
3. 重复 1 和 2

问：那 JVM 从哪里寻找并加载类呢？  
答：在 Classpath 中寻找

举个例子，如下我只是构建了一个非常简单的打印 hello 的 Java 程序：

```java
package com.github.hcsp.objectbasic;

public class Print {
    public static void main(String[] args) {
        String s = "Hello";
        System.out.println(s);
        s = s.toUpperCase();
        System.out.println(s);
    }
}
```

当我们在 idea 中按下 Run 时，idea 终端会显示如下信息：

```bash
"C:\Program Files\Java\jdk1.8.0_311\bin\java.exe" . . .
Hello
HELLO

Process finished with exit code 0
```

我们展开`"C:\Program Files\Java\jdk1.8.0_311\bin\java.exe" . . .`：

```bash
# 执行java命令
"C:\Program Files\Java\jdk1.8.0_311\bin\java.exe"

"-javaagent:C:\Program Files\JetBrains\IntelliJ IDEA Community Edition 2021.2.3\lib\idea_rt.jar=62985:C:\Program Files\JetBrains\IntelliJ IDEA Community Edition 2021.2.3\bin"

# 设置统属性
-Dfile.encoding=UTF-8

# 设置所引用的类库的位置
-classpath "
C:\Program Files\Java\jdk1.8.0_311\jre\lib\charsets.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\deploy.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\ext\access-bridge-64.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\ext\cldrdata.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\ext\dnsns.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\ext\jaccess.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\ext\jfxrt.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\ext\localedata.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\ext\nashorn.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\ext\sunec.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\ext\sunjce_provider.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\ext\sunmscapi.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\ext\sunpkcs11.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\ext\zipfs.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\javaws.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\jce.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\jfr.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\jfxswt.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\jsse.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\management-agent.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\plugin.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\resources.jar;
C:\Program Files\Java\jdk1.8.0_311\jre\lib\rt.jar;
C:\Users\Elite Fan\IdeaProjects\overload-constructors\target\classes"

 com.github.hcsp.objectbasic.Print
```

问题来了，我们只是运行了一个类，classpath 就已经是一长串引用了，当项目复杂的时候，所引用的类库里还会引用别的类库，层层引
用就会陷入类似回调地狱那样的状态，而且还有一个问题就是同样包的不同版本会被不同的库引用，这时我们应该引用哪个版本呢
？maven 的出现让这些问题得到极大的改善。

### maven

1. 约定大于配置，例如默认的 src/main src/test 目录
2. maven 中央仓库，按照 groupId/artifactId/version 的约定为所有包编号
3. maven 本地仓库放在~/.m2 文件夹中
4. 解决传递性依赖问题，不允许同名不同版本的包同时出现，依赖冲突就近选择

在命令行中使用`mvn dependency:tree`查看解决包冲突后的依赖树，maven 依赖冲突时使用就近选择方式  
例如如下 ABCD 四个包的依赖关系：  
A0.1>B0.2>C0.2  
D0.2>C0.1  
maven 最终会选择加载 C0.1 这个包，这就会出现新的问题，比如 C0.1 版本里缺失 C0.2 中新增的方法，运行时就会报错。

1. 解决方法 1：直接在 pom.xml 中引入你想要的版本  
   直接引入我想要的 C0.2 版本

```xml
<dependencies>
        <dependency>
            <groupId>AA.AAA.AAAA</groupId>
            <artifactId>A</artifactId>
            <version>0.1</version>
        </dependency>
        <dependency>
            <groupId>DD.DDD.DDDD</groupId>
            <artifactId>D</artifactId>
            <version>0.2</version>
        </dependency>
        <dependency>
            <groupId>CC.CCC.CCCC</groupId>
            <artifactId>C</artifactId>
            <version>0.2</version>
        </dependency>
</dependencies>
```

2. 解决方法 2：排除某个包的某个依赖(idea 中可以使用 maven helper 插件)

```xml
<dependencies>
        <dependency>
            <groupId>AA.AAA.AAAA</groupId>
            <artifactId>A</artifactId>
            <version>0.1</version>
        </dependency>
        <dependency>
            <groupId>DD.DDD.DDDD</groupId>
            <artifactId>D</artifactId>
            <version>0.2</version>
            <exclusions>
                <groupId>CC.CCC.CCCC</groupId>
                <artifactId>C</artifactId>
                <version>0.2</version>
            </exclusions>
        </dependency>
</dependencies>
```

maven 的 pom.xml 的 dependency 中的 scope 属性的作用

```xml

<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-api</artifactId>
    <version>5.6.0</version>
    <scope>test</scope>
</dependency>
```

scope 的取值：

1. test，仅在测试环境加载当前包
2. compile，编译和运行时都加载当前包
3. provided，仅在编译的时候使用当前包
