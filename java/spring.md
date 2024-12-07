### spring

- spring 容器，一个 IOC(Inversion of Control 控制反转)容器
- springMVC，基于 Spring 和 servlet 的 web 应用框架
- springBoot，集成度和自动化程度更高

#### spring 容器的核心概念

- Bean 容器中的最小工作单元，通常为一个 java 对象
- BeanFactory ApplicationContext
- 依赖注入（dependence injection）容器负责注入所有的依赖
- 控制翻转（inversion of control）用户将控制权交给容器

#### 手写一个简单的 spring 实现

**注意：** 为了简单起见，依然使用了 maven 依赖插件 springContext 的 Autowired 注
解接口来获取依赖：

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.20</version>
        </dependency>
    </dependencies>
```

1. 定义 Bean 配置文件：我们使用 Java 支持的古老的.properties 配置文件格式  
   在 src/main/resource/ioc.properties

```java
orderDao = com.github.hcsp.OrderDao
orderService = com.github.hcsp.OrderService
```

2. Bean 文件:

- src/mian/java/org/example/OrderDao

```java
package org.example;

public class OrderDao {
    public void select(){
        System.out.println("执行成功！");
    }
}
```

- src/mian/java/org/example/OrderService

```java
package org.example;

import org.springframework.beans.factory.annotation.Autowired;

public class OrderService {
    @Autowired
    private OrderDao orderDao;
    public void doSomeThing(){
        orderDao.select();
    }
}
```

3. 实例化 Bean，查找依赖，实现自动注入

- src/mian/java/org/example/MyIocContainer

```java
package org.example;

import org.springframework.beans.factory.annotation.Autowired;

import java.io.IOException;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class MyIocContainer {
    public static Map<String, Object> initBeans;

    public static void init() {
        Properties properties = new Properties();
        try {
            // 加载Bean配置文件（类映射文件）
            properties.load(MyIocContainer.class.getResourceAsStream("/ioc.properties"));
            System.out.println(properties);
            Map<String, Object> beans = new HashMap<>();
            // properties：{orderDao=org.example.OrderDao, orderService=org.example.OrderService}
            // 实例化配置文件中所有的 Bean 并放到Map集合 beans 中
            properties.forEach((beanName, fullyQualifiedClassName) -> {
                try {
                    Class klass = Class.forName((String) fullyQualifiedClassName);
                    Object beanInstance = klass.getConstructor().newInstance();
                    beans.put((String) beanName, beanInstance);
                } catch (ClassNotFoundException | InvocationTargetException | InstantiationException |
                         IllegalAccessException | NoSuchMethodException e) {
                    throw new RuntimeException(e);
                }
            });
            // 遍历beans中的实例化对象，使用反射获取对应实例的依赖
            beans.forEach((beanName, beanInstance) -> {
                dependencyInject(beanName, beanInstance, beans);
            });
            initBeans = beans;
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    private static void dependencyInject(String beanName, Object beanInstance, Map<String, Object> beans) {
        // 获取有Autowired注解的字段
        List<Field> fieldsTobeAutowired = Stream.of(beanInstance.getClass().getDeclaredFields())
                .filter(field -> field.getAnnotation(Autowired.class) != null)
                .collect(Collectors.toList());
        fieldsTobeAutowired.forEach(field -> {
            try {
                String fieldName = field.getName();
                // 在已经实例化所有所需要的bean对象的beans中查找依赖的那个实例对象
                Object depencyBeanInstance = beans.get(fieldName);
                field.setAccessible(true);
                // 将当前对象所依赖的对象注入
                field.set(beanInstance, depencyBeanInstance);
            } catch (IllegalAccessException e) {
                throw new RuntimeException(e);
            }
        });
    }

    public static void main(String[] args) {
        // 执行
        init();
        OrderService orderService = (OrderService) initBeans.get("orderService");
        OrderDao orderDao = (OrderDao) initBeans.get("orderDao");
    }
}
```
#### spring 是怎么实现的
- 在xml中定义Bean，或者使用注解定义Bean
- BeanDefinition的载入和解析
- Bean 的实例化和依赖注入
- 对外提供服务