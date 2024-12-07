## 概念

type：类型别名，用来给类型起个新名字  
interface：接口，用来定义对象的类型

## 相同点

```TypeScript
interface User {
  name: string
  age: number
  setUser(name: string, age: number): void
}

type User = {
  name: string
  age: number
  setUser(name: string, age: number): void
}

```
