## h5 页面嵌入 QQ 客服代码

```markup
<a href="mqqwpa://im/chat?chat_type=wpa&uin={{yourQQNumber}}&version=1&src_type=web&web_src=oicqzone.com"></a>
```

## 将一个数组分为多个数组

示例：[1,2,3,4,5,6,7,8,9,10] 转换为 [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10]]

```javascript
const sliceArray = function (targetArray, number) {
  const page = Math.ceil(targetArray.length / number);
  const returnArr = [];
  for (let i = 0; i < page; i++) {
    returnArr[i] = targetArray.slice(i * number, (i + 1) * number);
  }
  return returnArr;
};
```

## 限制只能传入数字

```javascript
// valStr 确保是一个字符串类型
const limitToNumberStr = function (valStr) {
  let tempValue = valStr.toString();
  return tempValue
    .replace(/[^0-9.]/g, "")
    .replace(".", "#*")
    .replace(/\./g, "")
    .replace("#*", ".");
};
```

## 将对象转换为数组

### 方式 1

示例：{ userName : 'elite' , local : 'china' } 转换为 [{ name : 'userName' , value : 'elite' } , { name : 'local' ,
value : 'china' }]

```javascript
const objTransToArray = function (obj) {
  let arr = [];
  Object.keys(obj).forEach((itemKey) => {
    let o = {};
    o.name = itemKey;
    o.value = obj[itemKey];
    arr.push(o);
  });
  return arr;
};
```

### 方式 2

示例：{ userName : 'elite' , local : 'china' } 转换为 [{ userName : 'elite' } , { local : 'china' }]

```javascript
const objTransToArray = function (obj) {
  let arr = [];
  Object.keys(obj).forEach((itemKey) => {
    let o = {};
    o[itemKey] = obj[itemKey];
    arr.push(o);
  });
  return arr;
};
```

## 判断数据类型

### 方法 1 返回值为 Boolean 值

```javascript
// yourData 传入的数据
// dataType 要验证的数据类型，例如 String Number
const checkType = function (yourData, dataType) {
  const type = Object.prototype.toString.call(yourData).slice(8, -1);
  return type === dataType;
};
```

### 方法 2 返回值为数据类型

```javascript
// checkdataType({}); // "object"
const checkdataType = function (yourData) {
  const type = Object.prototype.toString.call(yourData).slice(8, -1);
  return type.match(/\[object (.*?)\]/)[1].toLowerCase();
};
```

## 巧用 window.open

不想使用 a 标签破环样式结构的话，可以使用 window.open 实现部分常用功能  
`window.open(URL,name,specs,replace)`

```javascript
//  在当前页面显示拨打电话
window.open("tel:15000000000", "_self");
```

## js 获取 div 宽高

```javascript
const o = document.getElementById("view");
const height = o.offsetHeight;
const width = o.offsetWidth;
```

## js 时间处理

1. 方法 1：  
   传入一个合法的 date 格式时间数据，返回 {YY, MM, DD, HH, mm, ss}，默认返回当前时间的对象格式  
   例如：`formatDate(new Date())`返回对象：{ YY: '2021', MM: '01', DD: '12', HH: '09', mm: '56', ss: '55' }

```javascript
const formatDate = function (date = new Date()) {
  const YY = date.getFullYear() + "";
  const MM = (date.getMonth() + 1 < 10 ? "0" + (date.getMonth() + 1) : date.getMonth() + 1) + "";
  const DD = date.getDate() < 10 ? "0" + date.getDate() : date.getDate() + "";
  const HH = date.getHours() < 10 ? "0" + date.getHours() : date.getHours() + "";
  const mm = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes() + "";
  const ss = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds() + "";
  return { YY, MM, DD, HH, mm, ss };
};
```

2. 方法 2：  
   返回类似 "2021/01/12 09:48:04" 格式的时间字符串

```javascript
const formatNumber = (n) => {
  n = n.toString();
  return n[1] ? n : `0${n}`;
};
const formatTime = (date) => {
  const year = date.getFullYear();
  const month = date.getMonth() + 1;
  const day = date.getDate();
  const hour = date.getHours();
  const minute = date.getMinutes();
  const second = date.getSeconds();

  return `${[year, month, day].map(formatNumber).join("/")} ${[hour, minute, second].map(formatNumber).join(":")}`;
};
```

## js 获取前一天的日期

```javascript
const yestodayDate = function () {
  const today = new Date();
  const yestoday = today - 1000 * 60 * 60 * 24;
  const yestodayDate = new Date(yestoday);
  const res = yestodayDate.toLocaleDateString().replace(/\//g, "-");
  return res;
};
```

## js 获取某月最后一天是几号

```javascript
const getLastdayOfTheMonth = function (data) {
  let month = new Date(date).getMonth() + 1;
  let year = new Date(date).getFullYear();
  if (month > 11) {
    month -= 12;
    year++;
  }
  let nextDate = new Date(year, month, 1);
  return new Date(nextDate.getTime() - 1000 * 60 * 60 * 24).getDate();
};
```

## 更新对象属性值

在不给目标对象增加新的属性的同时更新目标对象属性值

```javascript
const updateObj = function (target, source) {
  const keys = Object.keys(target);
  // const merge = Object.assign(target, source) 此方法会直接改变 target 对象
  const merge = { ...target, ...source };
  const temp = {};
  keys.map((item, index) => {
    temp[item] = merge[item];
  });
  return temp;
};
```

## 删除数组某一项

index 从 0 开始

```javascript
const deleteItem = function (index, arr) {
  arr.splice(index, 1);
};
```

## 获取文件类型

```javascript
//  方法1
const fileType = fileName.substring(fileName.lastIndexOf(".") + 1);
//  方法2
const fileType = fileName.split(".").pop().toLowerCase();
```

## 复制文字

```javascript
function copy(targetValue) {
  let transfer = document.createElement("input");
  document.body.appendChild(transfer);
  transfer.value = targetValue; // 这里表示想要复制的内容
  transfer.focus();
  transfer.select();
  if (document.execCommand("copy")) {
    document.execCommand("copy");
  }
  transfer.blur();
  document.body.removeChild(transfer);
}
```

## 数组去重

```javascript
function uniqueArr(arr) {
  return Array.from(new Set(arr));
}

let tempArr = [1, 1, 2, 3, "3", 4, "4", 5, 5, 5, 6];
const resArr = uniqueArr(tempArr);
console.log(resArr); // [1, 2, 3, "3", 4, "4", 5, 6]
```

## 数组取交集/差集

```javascript
// 取交集
function intersection(a, b) {
  return a.filter((v) => b.includes(v));
}
// 取差集
function difference(a, b) {
  return a.concat(b).filter((v) => !a.includes(v) || !b.includes(v));
}
const a = [1, 2, 3, 4, 5];
const b = [1, 2, 3, 7, 8, 9];
intersection(a, b); // [1,2,3]
difference(a, b); // [4,5,7,8,9]
```

## 数组 a 是否包含数组 b

```javascript
arrayIncludes(a,b){
  return b.every(val=>a.includes(val));
}
```

## 下划线转驼峰命名

```javascript
function tuHump(name) {
  return name.toLowerCase().replace(/\_(\w)/g, function (all, letter) {
    return letter.toUpperCase();
  });
}
```

## 滚动到页面顶部、底部

```js
// 返回顶部
const scrollToTop = () => {
  const targetDom = document.getElementById("scroll-wrap");
  if (targetDom) {
    targetDom.scrollTop = 0;
  }
};

// 返回底部
const scrollToBottom = () => {
  const targetDom = document.getElementById("scroll-wrap");
  targetDom.scrollTop = targetDom.scrollHeight;
};
const scrollToBottom = () => {
  const targetDom = document.getElementsByClassName("scroll-wrap");
  for (let i = 0; i < targetDom.length; i++) {
    targetDom[i].scrollTop = targetDom[i].scrollHeight;
  }
};
```

## 滚动到某个子元素的位置

```html
<div id="parent">
  <div class="child" id="child1">1</div>
  <div class="child" id="child2">2</div>
  <!-- ...... -->
  <div class="child" id="child9">9</div>
  <div class="child" id="child10">10</div>
</div>
<script>
  const parent = document.getElementById("parent");
  const child = document.getElementById("child4");
  parent.scrollTo({
    top: child.offsetTop, //需要父元素设置postion(relative、absolute、fixed)
    behavior: "smooth",
  });
</script>
```

## 获取两个元素的纵向距离

```js
let ele1Top = document.getElementById("ele1")?.getBoundingClientRect().top;
let ele2Top = document.getElementById("ele2")?.getBoundingClientRect().top;
const distance = ele2Top - ele1Top;
```

## 防抖和节流

函数防抖（debounce）：  
当持续触发事件时，一定时间段内没有再触发事件，事件处理函数才会执行一次，如果设定的时间到来之前，又一次触发了事件，就重新
开始延时。

```js
// 防抖函数
function debounce(func, wait) {
  let timeout = null;
  return function () {
    if (timeout !== null) clearTimeout(timeout);
    timeout = setTimeout(func, wait);
  };
}
// 处理函数
function handle() {
  console.log(Math.random());
}
// 滚动事件
window.addEventListener("scroll", debounce(handle, 1000));
```

函数节流（throttle）：  
当持续触发事件时，保证一定时间段内只调用一次事件处理函数。

```js
// 使用时间戳实现节流
function throttle(func, delay) {
  let prev = Date.now();
  return function () {
    let context = this;
    let args = arguments;
    let now = Date.now();
    if (now - prev >= delay) {
      func.apply(context, args);
      prev = Date.now();
    }
  };
}
function handle() {
  console.log(Math.random());
}
// 快速点击浏览器进行测试
window.addEventListener("click", throttle(handle, 1000));

// 使用定时器实现节流
function throttle(func, delay) {
  let timer = null;
  return function () {
    let context = this;
    let args = arguments;
    if (!timer) {
      timer = setTimeout(function () {
        func.apply(context, args);
        timer = null;
      }, delay);
    }
  };
}
function handle() {
  console.log(Math.random());
}
// 快速点击浏览器进行测试
window.addEventListener("click", throttle(handle, 1000));
```
