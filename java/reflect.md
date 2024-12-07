### java 反射

#### Class 类

java 运行时系统始终为所有对象维护一个运行时类型标识，这个信息就是类的说明书，会
跟踪每一个对象所属的类。保存这些信息的类名为 Class。

获取某个类的 Class 类对象：

```java
// getClass 方法返回类名对应的Class对象
Employee e = new Employee();
Class cl = e.getClass();

// 使用静态方法forName获得类名对应的Class对象
String className = "java.util.Random";
Class cl = Class.forName(className);

// 如果T是任意的java类，T.class 将是匹配的类对象
Class cl1 = Random.class;
Class cl2 = int.class;
Class cl3 = Double[].class;
```

Class 类也是一个泛型类：Emplyee.class 的类型是 Class<Employee>

#### 使用 Class 类查找获取资源的功能

- 什么是资源文件：一般指除了.java 和.class 结尾的文件，都是资源文件，例如图像和
  声音文件。

在一个拥有资源文件的类的 Class 对象 ResourceTest 中

```java
Class cl = ResourceTest.class;
URL aboutURL = cl.getResource("about.gif");
var icon = new ImageIcon(aboutURL);

InputStream stream = cl.getResourceAsStream("data/about.txt");
var about = new String(stream.readAllBytes(),"UTF-8");
```

- 一个实例

```java
public class MapBeanConverter {
    // 传入一个遵守Java Bean约定的对象，读取它的所有属性，存储成为一个Map
    // 例如，对于一个DemoJavaBean对象 { id = 1, name = "ABC" }
    // 提示：
    //  1. 读取传入参数bean的Class
    //  2. 通过反射获得它包含的所有名为getXXX/isXXX，且无参数的方法（即getter方法）
    //  3. 通过反射调用这些方法并将获得的值存储到Map中返回
    public static Map<String, Object> beanToMap(Object bean) {
        Map<String, Object> map = new HashMap<>();
        Class beanClass = bean.getClass();
        Method[] methods = beanClass.getDeclaredMethods();
        for (Method method :
                methods) {
            String methodName = method.getName();
            Boolean isStartWithGetorIs = methodName.startsWith("get") || methodName.startsWith("is");
            int methodParameterNumber = method.getParameterTypes().length;
            if (methodParameterNumber == 0 && isStartWithGetorIs) {
                try {
                    Object obj = beanClass.getMethod(methodName).invoke(bean);
                    map.put(methodName, obj);
                } catch (IllegalAccessException | InvocationTargetException | NoSuchMethodException e) {
                    throw new RuntimeException(e);
                }
            }
        }
        return map;
    }

    // 传入一个遵守Java Bean约定的Class和一个Map，生成一个该对象的实例
    // 传入参数DemoJavaBean.class和Map { id -> 1, name -> "ABC"}
    // 应当返回一个DemoJavaBean对象 { id = 1, name = "ABC" }
    // 提示：
    //  1. 遍历map中的所有键值对，寻找klass中名为setXXX，且参数为对应值类型的方法（即setter方法）
    //  2. 使用反射创建klass对象的一个实例
    //  3. 使用反射调用setter方法对该实例的字段进行设值
    public static <T> T mapToBean(Class<T> klass, Map<String, Object> map) {
        try {
            Object klassInstance = klass.getConstructor().newInstance();
            for (String key :
                    map.keySet()) {
                Field declaredField = klass.getDeclaredField(key);
                declaredField.setAccessible(true);
                declaredField.set(klassInstance,map.get(key));
            }
            return (T) klassInstance;
        } catch (InstantiationException | IllegalAccessException | InvocationTargetException |
                 NoSuchMethodException | NoSuchFieldException e) {
            throw new RuntimeException(e);
        }
    }

    public static void main(String[] args) {
        DemoJavaBean bean = new DemoJavaBean();
        bean.setId(100);
        bean.setName("AAAAAAAAAAAAAAAAAAA");
        System.out.println(beanToMap(bean));

        Map<String, Object> map = new HashMap<>();
        map.put("id", 123);
        map.put("name", "ABCDEFG");
        System.out.println(mapToBean(DemoJavaBean.class, map));
    }

    public static class DemoJavaBean {
        private Integer id;
        private String name;
        private String privateField = "privateField";

        public int isolate() {
            System.out.println(privateField);
            return 0;
        }

        public String is() {
            return "";
        }

        public Integer getId() {
            return id;
        }

        public void setId(Integer id) {
            this.id = id;
        }

        public String getName() {
            return name;
        }

        public String getName(int i) {
            return name + i;
        }

        public void setName(String name) {
            this.name = name;
        }

        public boolean isLongName() {
            return name.length() > 10;
        }

        @Override
        public String toString() {
            return "DemoJavaBean{"
                    + "id="
                    + id
                    + ", name='"
                    + name
                    + '\''
                    + ", longName="
                    + isLongName()
                    + '}';
        }
    }
}
```
