## @types

它指的是 [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)，这个仓库中记录了 90% 的顶级 JavaScript 库  
例如，jQuery 是用 js 写的，不支持 Ts 类型检查，在项目中，你可以安装`npm install -D @typesjquery`来让 jQuery 支持 Ts 类型检查。

@types 的本质是用来存放类型声明文件（\*.d.ts）的库，常用来为 **非天生支持 Ts** 的库提供类型检查支持。

## ?? ?: ?. !.的使用

### ?? 和 js 中的 || 相似，但是比 || 严谨

```typescript
console.log(null || 1) // 1
console.log(undefined || 1) // 1
console.log(2 || 1) // 2
console.log(0 || 1) // 1

console.log(null ?? 1) // 1
console.log(undefined ?? 1) // 1
console.log(2 ?? 1) // 2
console.log(0 ?? 1) // 0
```

我们发现 0||1 返回的是 1，而 0??1 返回的是 0

### ?:就比较常用了，指可选参数

### TypeScript 3.7 实现了?.(可选链)

如果遇到 null 或 undefined 就可以立即停止某些表达式的运行  
a?.b 表示：先检查 a 是否为 null 或 undefined，若是，则直接返回 undefned，若不是，则取 a.b

```typescript
const a = {
  b: 4,
  d: 5
}
const val = a?.b

// 转换成js则变成

var a = {
  b: 4,
  d: 5
}
var val = a === null || a === void 0 ? void 0 : a.b
```

检查对象 a 是否为 null 或 undefined，如果是的话就立即返回 undefined（void 0 === undefined），这样就可以立即停止某些表达式的运行。

?. 与 && 运算符行为略有不同，&& 专门用于检测 falsy 值，比如空字符串、0、NaN、null 和 false 等。而 ?. 只会验证对象是否为 null 或 undefined，对于 0 或空字符串来说，并不会出现 “短路
”。

### !.的意思是断言，告诉 ts 你这个对象里一定有某个值

## as const 技巧

当使用 as const 断言：

1. 不会扩大表达式中的文字类型
2. 对象中的属性变为只读
3. 数组中的元素变为只读元组

举例说明：

```javascript
// Type '"hello"'
let x = "hello" as const;
// Type '"string"'
let x = "hello";

// Type 'readonly [10, 20]'
let y = [10, 20] as const;
// Type 'number[]'
let y = [10,20]

// Type '{ readonly text: "hello" }'
let z = { text: "hello" } as const;
// Type '{text: string;}'
let z = { text: "hello" };

```
