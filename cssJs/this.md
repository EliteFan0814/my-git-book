## javaScript 中的 this

#### this 的值

1. 非严格模式下，this 总是指向一个对象
2. 严格模式下，this 可以是任意值
3. 无论是否在严格模式下，在全局执行环境中（在任何函数体外部）this 都指向全局对象

#### this 到底指向谁

和 this 相关的函数的常用调用方式：

1. 纯粹的函数调用
2. 作为对象方法的调用
3. call 调用 或 apply 调用
4. 作为构造函数调用

来看一道题：

```javascript
var x = 0;
var y = 1;
// 普通函数
function test() {
  console.log(this.x);
}
// 构造函数
function Test2() {
  this.y = 2;
}
var obj = {
  x: 1,
  test: test,
};
var obj2 = new Test2();

var arr = [test, new Test2()];

// ----- 以下各调用方式中的this值就是call()中的第一个参数 -----

// 纯粹的函数调用
test(); // 打印 0 该式可看作：test.call(undefined)

// 作为对象方法的调用
obj.test(); // 打印 1 该式可看作：obj.test.call(obj)

// call 调用 或 apply 调用
obj.test.call(); // 打印 0 该式可看作：obj.test.call(undefined)
obj.test.call(obj); // 打印 1

// 作为构造函数调用
console.log(obj2.y); // 打印 2 该式可看作：obj2.y.call(obj2)

// 在数组中
arr[0](); // 打印 undefined 该式可看作 arr.0.call(arr)
arr[1].y; // 值为 2 该式可看作 arr[1].y.call(arr[1])
```

以上我们最常见到的是类似 `test(arg1,arg2, ...)` `obj.test(arg1,arg2, ...)` 这样
的调用方式，而很少使用类似 `obj.test.call(thisArg,arg1,arg2, ...)` 这样的调用方
式。所以才会搞不清楚 this 到底指向了哪里。

`test(arg1,arg2, ...)` 等价于 `test.call(undefined,arg1,arg2, ...)`  
`obj.test(arg1,arg2, ...)` 等价于`obj.test.call(obj,arg1,arg2, ...)`  
所以总结起来都是在以 `func.call(thisArg,arg1,arg2, ...)` 这样的的方式调用函数。

那么 this 所指向的值就很明了了：

** this 的值，就是 `call(thisArg,arg1,arg2, ...)` 里的 thisArg 的值。**

call 函数和 apply 函数用法类似，所以这里就不讨论 apply 了。

需要注意的是：call 函数中指定的 this 值并不一定是该函数执行时真正的 this 值，如
果这个函数处于 non-strict mode 模式，则指定为 null 和 undefined 的 this 值会自动
指向全局对象(浏览器中就是 window 对象)，同时值为原始值(数字，字符串，布尔值)的
this 会指向该原始值的自动包装对象（严格模式下默认 thisArg 是 undefined）。

#### ES6 中箭头函数与 this

先看个示例（不建议在定义对象的方法时使用箭头函数，这里只是为了探讨问题而使用）：

```javascript
var obj = {
  name: "fpc",
  tool: () => {
    console.log(this);
  },
  tool2: function () {
    console.log(this);
  },
};
obj.tool(); // 打印 window 对象 相当于 obj.tool.call(obj)
obj.tool2(); // 打印 obj 对象 相当于 obj.tool2.call(obj)
obj.tool.call(); // 打印 window 对象  obj.tool.call(undefined)
obj.tool2.call(); // 打印 window 对象  obj.tool2.call(undefined)
obj.tool.call(obj); // 打印 window 对象
obj.tool2.call(obj); // 打印 obj 对象
```

tool 方法用箭头函数定义，tool2 方法用普通函数定义。

从 `obj.tool.call(obj) // 打印 window 对象` 可以看出，即使用 call 指定了 this 值
为 obj，打印出来的依然是 window 对象。而 `obj.tool2.call(obj) // 打印 obj 对象`
就正常输出 call 指定的 obj 对象。

阮一峰老师的 ES6 教程里是这样解释的： **箭头函数内的 this 对象，就是定义时所在的
对象，而不是使用时所在的对象。**

回到这个例子中，就是说 箭头函数 tool 定义时所在的作用域为全局作用域，所以它里面
的 this 指向 window 。这里会有很多人不能理解，tool 不是 obj 中定义的方法吗？怎么
会指向 window 。我最开始也一脸迷茫，想了好久才明白：

this 值的判断条件并 **不是对象**，而是 **所在作用域** ，js 只有 **函数作用域**
和 **全局作用域** （这里先说 ES5，ES6 暂不讨论）所以可以这样理解： **箭头函数中
必须通过查找作用域来确定其 this 值，简单来说就是：箭头函数中 this 绑定的是距它最
近一层非箭头函数作用域的 this 值。**

箭头函数 tool 往外一层就是全局作用域了，所以 tool 中的 this 就是 window 。

这样理解还是有点复杂，再简化一下：**箭头函数里没有 this ，如果里面出现 this ，就
向上查找最近的 this 值来使用。**

再来看一个例子：

```javascript
var obj = {
  props: 37,
  foo: () => {
    return this.props;
  },
  foo2: function () {
    return this.props;
  },
  bar: function () {
    var x = () => this;
    return x;
  },
  bar2: function () {
    "use strict";
    var x = () => this;
    return x;
  },
};

// obj.foo()≈obj.foo.call(obj) 因为obj.foo是箭头函数，所以 this 是它往上一层非箭头函数作用域，
// 即全局作用域 window
//  window 没有 props 属性，所以返回 undefined
console.log(obj.foo()); // undefined

// obj.foo2()≈obj.foo2.call(obj) this 则为 obj obj.props 是37
console.log(obj.foo2()); // 37

var fn = obj.bar();
// 距离箭头函数 x 最近的非箭头函数作用域是 obj.bar()
// 再根据 obj.bar()≈obj.bar.call(obj) 得出 x 中的 this 是 obj
console.log(fn() === obj); // true

var fn2 = obj.bar;
// 距离箭头函数 x 最近的非箭头函数作用域是 fn2()
// 再根据 fn2()≈fn2.call(undefined) 得出 x 中的 this 是 window (非严格模式下)
console.log(fn2()() == window); // true

var fn2 = obj.bar2;
// 距离箭头函数 x 最近的非箭头函数作用域是 fn2()
// 再根据 fn2()≈fn2.call(undefined) 得出 x 中的 this 是 undefined (严格模式下)
console.log(fn2()() == undefined); // true
```

再看一个例子：

```javascript
var o = { prop: 37 };

function independent() {
  return this.prop;
}

let independent2 = () => {
  return this.prop;
};
o.f = independent;
o.f2 = independent2;

console.log(o.f()); // 37
console.log(o.f2()); // undefined
```

#### 类中的 this

和其他普通函数一样，类的实例的方法中的 this 值取决于它们如何被调用，有时，改写这
个行为，让类中的 this 值总是指向这个类实例会很有用。为了做到这一点，可在构造函数
中绑定类方法。

仔细看下题：

```javascript
class Car {
  constructor() {
    this.type = "实例的车";
    this.boot = this.boot.bind(this);
  }
  boot() {
    console.log("原型方法：启动！type是：" + this.type);
  }
  boot2() {
    console.log("原型方法：启动！type是：" + this.type);
  }
  boot3 = () => {
    console.log("原型方法：启动！type是：" + this.type);
  };
  static trademark() {
    console.log("每辆车都有自己的品牌");
  }
}

let car = new Car();
let boot = car.boot;
let boot2 = car.boot2;
let boot3 = car.boot3;

// boot 绑定 this 到实例上，所以两种调用方式 this 都指向实例对象
car.boot(); // "原型方法：启动！type是：实例的车"
boot(); //  "原型方法：启动！type是：实例的车"

// boot2 未绑定 this 到实例上，
// 第一个的 this 指向实例对象
// 第二个的 this 指向undefined，又因为 class 是严格模式下执行的，所以undefined不会转向 window 从而报错
car.boot2(); //  "原型方法：启动！type是：实例的车"
boot2(); //  "TypeError: Cannot read properties of undefined (reading 'type')"

// boot3用箭头函数定义
car.boot3(); // "原型方法：启动！type是：实例的车"
boot3(); //  "原型方法：启动！type是：实例的车"
```
