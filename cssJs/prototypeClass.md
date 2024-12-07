## JavaScript 构造函数与 class 类

#### 构造函数

```javascript
function Star(name, age) {
  this.name = name;
  this.age = age;
  this.sing = function () {
    console.log(`${name}在唱歌`);
  };
}
let 周杰伦 = new Star("周杰伦", 22);
let 蔡徐坤 = new Star("蔡徐坤", 20);
let 张韶涵 = new Star("张韶涵", 18);
let 张惠妹 = new Star("张惠妹", 28);

周杰伦.sing(); // "周杰伦在唱歌"
蔡徐坤.sing(); // "蔡徐坤在唱歌"
张韶涵.sing(); // "张韶涵在唱歌"
张惠妹.sing(); // "张惠妹在唱歌"
```

#### 构造函数存在的问题

上例存在一个问题：每一个实例都存在 sing 方法，这会占据大量的内存空间，造成内存浪
费。 我们希望所有的对象使用同一个函数，这样就比较节省内存，那么我们要怎样做呢？

```javascript
function Star(name, age) {
  this.name = name;
  this.age = age;
}
Star.prototype.sing = function () {
  console.log(`${name}在唱歌`);
};
```

JavaScript 规定，每一个构造函数都有一个 prototype 属性，指向另一个对象（原型对象
）以上例子中，构造函数通过原型 prototype 分配的函数是所有对象所共享的。我们可以
把那些通用的方法，直接定义在 prototype 对象上，这样所有对象的实例就可以共享这些
方法。

如果有多个对象的方法，我们可以给原型对象采取对象形式赋值：

```javascript
function Star(name, age) {
  this.name = name;
  this.age = age;
}

Star.prototype = {
  // 如果修改了原来的原型对象，给原型对象赋值的是一个对象，则必须手动利用 constructor 指回原来的构造函数
  constructor: Star,
  sing: function () {},
  move: function () {},
  eat: function () {},
  sleep: function () {},
};
```

#### 什么是原型链，看我制作的图

![js原型链.jpg](https://s2.loli.net/2022/11/03/lCD4PRX21upmxoe.png)

简单来说就是，每一个对象都有一个原型，每一个原型又是一个对象，所以原型又有自己的
原型，这样一环扣一环形成一条链，就叫原型链。

#### JavaScript 的成员查找机制

1. 当访问一个对象的属性（包括方法）时，首先查找这个对象自身有没有该属性。
2. 如果没有就查找它的原型（也就是 **proto** 指向的 prototype 原型对象）。
3. 如果还没有就查找原型对象的原型（Object 的原型对象）。
4. 依此类推一直找到 Object 为止（null）。
5. **proto** 对象原型的意义就在于为对象成员查找机制提供一个方向，或者说一条路线
   。

#### 原型对象中的 this 指向 [详解 this](https://fan552426811.gitbook.io/front-end-tools/cssjs/this)

原型对象中定义的方法（函数）内**this 的指向**为这个方法（函数）的调用者，也就是
实例对象

```javascript
class Xing {
  constructor(name, age) {
    this.name = name;
    this.age = age;
    this.innerThis = function () {
      return this;
    };
  }
  outerThis() {
    return this;
  }
}

const zxc = new Xing("周星驰", 18);
console.log(zxc.innerThis() === zxc.outerThis()); // true
```

#### 构造函数与 class 类

如下可查看二者在构造上的区别：

```javascript
// 使用构造函数
function Star(name, age) {
  this.name = name;
  this.age = age;
}

Star.prototype = {
  constructor: Star,
  sing: function () {
    console.log(this.name + "会唱歌");
  },
  move: function () {
    console.log(this.name + "会移动");
  },
};

// 使用class类
class Star {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  sing() {
    console.log(this.name + "会唱歌");
  }
  move() {
    console.log(this.name + "会移动");
  }
}
```
