## TypeScript interface 接口

接口声明是命名对象类型的另一种方式，是一系列抽象方法的声明，接口定义的方法需要具体的类来实现。  
接口一般首字母大写。有的编程语言中会建议接口的名称加上 I 前缀。

```typescript
interface IPerson {
  readonly id: number // 只读属性
  firstName: string // 必传属性
  lastName: string
  age?: number // 可选属性
  talk: () => void
  run: string | string[] | (() => string) // 联合类型
  [propName: string]: any // 任意属性
}

const person: IPerson = {
  id: 1,
  firstName: '屁虫',
  lastName: '放',
  run: '我会跑步',
  talk() {
    console.log(`我的名字是${this.lastName}${this.firstName}`)
  },
  dance() {
    console.log('我会跳舞')
  }
}
person.talk()
```

**赋值的时候，变量的形状必须和接口的形状保持一致，属性不能多也不能少，不然会编译时报错**  
**一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**

### 类实现接口

```typescript
// 报警器
interface Alarm {
  // 报警功能
  alert(): void
  // 监视功能
  watch(): void
}
// 灯光功能
interface Light {
  lightOn(): void
  lightOff(): void
}
// 创建 门 类，实现 报警器 功能
class Door implements Alarm {
  public static className = '门'
  public name: string
  constructor(name: string) {
    this.name = name
  }
  public alert(): void {
    console.log('我会报警功能')
  }
  public watch(): void {
    console.log('我会监视功能')
  }
}

const door: Door = new Door('盼盼安全门')
door.alert()
console.log(Door.className)

// 创建 汽车 类，也实现 报警器 功能
class Car implements Alarm, Light {
  public name: string
  constructor(name: string) {
    this.name = name
  }
  public alert(): void {
    console.log('我会报警功能')
  }
  public watch(): void {
    console.log('我会监视车辆安全')
  }
  public lightOn(): void {
    console.log('打开灯光')
  }
  public lightOff(): void {
    console.log('关闭灯光')
  }
}
```

### 接口继承接口

```typescript
interface Alarm {
  alert(): void
}

interface LightableAlarm extends Alarm {
  lightOn(): void
  lightOff(): void
}
```

### 接口继承类

TypeScript 中接口也可以继承类，因为声明一个类的同时，也会创建和类同名的一个类型，只不过同名类型不包括类中的构造函数、静态属性、静态方法：

```typescript
// 声明一个类
class Point {
  x: number
  y: number
  constructor(x: number, y: number) {
    this.x = x
    this.y = y
  }
}
// 同时会暗地里创建一个同名的类型
interface PointInstanceType {
  x: number
  y: number
}
```
