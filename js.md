---
description: JS常用函数集合
---

# JS常用

## h5 页面嵌入QQ客服代码

```markup
<a href="mqqwpa://im/chat?chat_type=wpa&uin={{yourQQNumber}}&version=1&src_type=web&web_src=oicqzone.com"></a>
```

## 将一个数组分为多个数组

示例：\[1,2,3,4,5,6,7,8,9,10\] =&gt; \[\[1, 2, 3, 4\], \[5, 6, 7, 8\], \[9, 10\]\]

```javascript
const sliceArray = function(targetArray,number){
   const page = Math.ceil(targetArray.length / number)
   const returnArr = []
   for (let i = 0; i < page; i++) {
     returnArr[i] = targetArray.slice(i * number, (i + 1) * number)
   }
  return returnArr
}
```

## 限制只能传入数字

```javascript
// valStr 确保是一个字符串类型
const limitToNumberStr = function(valStr) {
  let tempValue = valStr.toString()
  return tempValue
    .replace(/[^0-9.]/g, '')
    .replace('.', '#*')
    .replace(/\./g, '')
    .replace('#*', '.')
}
```

## 将对象转换为数组

### 方式1

示例：{userName:'elite',local:'china'} =&gt;\[{name:'userName',value:'elite'},{name:'local',value:'china'}\]

```javascript
const objTransToArray = function(obj){
   let arr = [];
   Object.keys(obj).forEach(itemKey => {
     let o = {};
     o.name = itemKey
     o.value = obj[itemKey]
     arr.push(o)
   })
    return arr
}
```

### 方式2

示例：{userName:'elite',local:'china'} =&gt;\[{userName:'elite'},{local:'china'}\]

```javascript
const objTransToArray = function(obj){
   let arr = [];
   Object.keys(obj).forEach(itemKey => {
     let o = {};
     o[itemKey] = obj[itemKey]
     arr.push(o)
   })
    return arr
}
```

## 判断数据类型

### 方法1 返回值为Boolean值

```javascript
// yourData 传入的数据
// dataType 要验证的数据类型，例如 String Number
const checkType = function (yourData,dataType) {
  const type = Object.prototype.toString.call(yourData).slice(8, -1)
  return type === dataType
}
```

### 方法2 返回值为数据类型

```javascript
const checkdataType = function (yourData) {
  const type = Object.prototype.toString.call(yourData).slice(8, -1)
  switch (type) {
    case 'Number':
      return 'Number'
    case 'String':
      return 'String'
    case 'Boolean':
      return 'Boolean'
    case 'Null':
      return 'Null'
    case 'Undefined':
      return 'Undefined'
    case 'Function':
      return 'Function'
    case 'Array':
      return 'Array'
    case 'Object':
      return 'Object'
    case 'Date':
      return 'Date'
    case 'RegExp':
      return 'RegExp'
    case 'Error':
      return 'Error'
    case 'Symbol':
      return 'Symbol'
    case 'Promise':
      return 'Promise'
    case 'Set':
      return 'Set'
    default:
      return '未知类型：' + type
  }
}
```

