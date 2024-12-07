## exports 与 module.exports

#### 从模块编写角度来看，二者等价

```javascript
console.log(exports)
console.log(module.exports)
console.log(module.exports === exports)

//  打印结果为：

// {}
// {}
// true
```

#### 从使用者角度，二者有区别

```javascript
// 使用 module.exports 语法写模块 exp.js
module.exports = function(){
  console.log('这是用module.exports写的模块')
}

// 引用时用如下方式引用
const expFunc = require('./exp.js)


// 使用 exports 语法写模块 exp.js
exports.expFunc = function(){
  console.log('这是用exports写的模块')
}

// 引用时使用者必须知道该函数的名称
const {expFunc} = require('./exp.js)
```

看另外一个例子

```javascript
// 模块 exp1.js
exports = 3.14

// 模块引用
const a = require('./exp1.js')
console.log(a)
//  打印结果：{}


// 模块 exp2.js
module.exports = 3.14

// 模块引用
const a = require('./exp2.js')
console.log(a)
//  打印结果：3.14
```

#### 总结

在 Node.js 模块里，真正控制模块导出的是 module.exports，exports 只是 module.exports 决定导出一个对象时的一个快捷方式，假
如 module.exports 导出其它类型的数据，比如字符串、数值、函数等等，则 exports 的存在没有意义。这是 Node.js 模块与
CommonJS 差异的一点，在 CommonJS 规范里，是只有 exports，没有 module.exports 的。

## 参考博文

> [区别 module.exports 与 exports](https://blog.zfanw.com/differences-between-exports-module-dot-exports/)
