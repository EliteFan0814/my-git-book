## h5 页面嵌入 QQ 客服代码

```markup
<a href="mqqwpa://im/chat?chat_type=wpa&uin={{yourQQNumber}}&version=1&src_type=web&web_src=oicqzone.com"></a>
```

## 将一个数组分为多个数组

示例：[1,2,3,4,5,6,7,8,9,10] 转换为 [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10]]

```javascript
const sliceArray = function (targetArray, number) {
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
const limitToNumberStr = function (valStr) {
  let tempValue = valStr.toString()
  return tempValue
    .replace(/[^0-9.]/g, '')
    .replace('.', '#*')
    .replace(/\./g, '')
    .replace('#*', '.')
}
```

## 将对象转换为数组

### 方式 1

示例：{ userName : 'elite' , local : 'china' } 转换为 [{ name : 'userName' , value : 'elite' } , { name : 'local' ,
value : 'china' }]

```javascript
const objTransToArray = function (obj) {
  let arr = []
  Object.keys(obj).forEach((itemKey) => {
    let o = {}
    o.name = itemKey
    o.value = obj[itemKey]
    arr.push(o)
  })
  return arr
}
```

### 方式 2

示例：{ userName : 'elite' , local : 'china' } 转换为 [{ userName : 'elite' } , { local : 'china' }]

```javascript
const objTransToArray = function (obj) {
  let arr = []
  Object.keys(obj).forEach((itemKey) => {
    let o = {}
    o[itemKey] = obj[itemKey]
    arr.push(o)
  })
  return arr
}
```

## 判断数据类型

### 方法 1 返回值为 Boolean 值

```javascript
// yourData 传入的数据
// dataType 要验证的数据类型，例如 String Number
const checkType = function (yourData, dataType) {
  const type = Object.prototype.toString.call(yourData).slice(8, -1)
  return type === dataType
}
```

### 方法 2 返回值为数据类型

```javascript
// checkdataType({}); // "object"
const checkdataType = function (yourData) {
  const type = Object.prototype.toString.call(yourData).slice(8, -1)
  return type.match(/\[object (.*?)\]/)[1].toLowerCase()
}
```

## 巧用 window.open

不想使用 a 标签破环样式结构的话，可以使用 window.open 实现部分常用功能  
`window.open(URL,name,specs,replace)`

```js
//  在当前页面显示拨打电话
window.open('tel:15000000000', '_self')
```

## 当前页点击下载

有时候后台管理需要导出功能，a 标签默认会打开新页面，此方法使用 dom 操作在当前页面创建下载链接并点击下载

```js
 //下载任务文件事件 url 为下载地址
    downloadFiles(url, name) {
      const aLink = document.createElement('a') //创建a链接
      aLink.style.display = 'none'
      aLink.href = url
      aLink.download = name
      document.body.appendChild(aLink)
      aLink.click()
      document.body.removeChild(aLink) //点击完成后记得删除创建的链接
    },
 //当后台返回的不是下载地址，而是文件的 blob 格式对象时
 //注意 blobObj 为 blob 格式对象！！！
 downloadFiles(blobObj,fileName){
   const tempBlob = URL.createObjectURL(blobObj)
   const aLink = document.createElement('a')
   aLink.href = tempBlob
   aLink.download = fileName
   aLink.click()
   URL.revokeObjectURL(tempBlob)
 }
```

## js 获取 div 宽高

```js
const o = document.getElementById('view')
const height = o.offsetHeight
const width = o.offsetWidth
```

## js 时间处理

1. 方法 1：  
   传入一个合法的 date 格式时间数据，返回 {YY, MM, DD, HH, mm, ss}，默认返回当前时间的对象格式  
   例如：`formatDate(new Date())`返回对象：{ YY: '2021', MM: '01', DD: '12', HH: '09', mm: '56', ss: '55' }

```js
const formatDate = function (date = new Date()) {
  const YY = date.getFullYear() + ''
  const MM = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + ''
  const DD = date.getDate() < 10 ? '0' + date.getDate() : date.getDate() + ''
  const HH = date.getHours() < 10 ? '0' + date.getHours() : date.getHours() + ''
  const mm = date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes() + ''
  const ss = date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds() + ''
  return { YY, MM, DD, HH, mm, ss }
}
```

2. 方法 2：  
   返回类似 "2021/01/12 09:48:04" 格式的时间字符串

```js
const formatNumber = (n) => {
  n = n.toString()
  return n[1] ? n : `0${n}`
}
const formatTime = (date) => {
  const year = date.getFullYear()
  const month = date.getMonth() + 1
  const day = date.getDate()
  const hour = date.getHours()
  const minute = date.getMinutes()
  const second = date.getSeconds()

  return `${[year, month, day].map(formatNumber).join('/')} ${[hour, minute, second].map(formatNumber).join(':')}`
}
```

## js 获取前一天的日期

```js
const yestodayDate = function () {
  const today = new Date()
  const yestoday = today - 1000 * 60 * 60 * 24
  const yestodayDate = new Date(yestoday)
  const res = yestodayDate.toLocaleDateString().replace(/\//g, '-')
  return res
}
```

## js 获取某月最后一天是几号

```js
const getLastdayOfTheMonth = function (data) {
  let month = new Date(date).getMonth() + 1
  let year = new Date(date).getFullYear()
  if (month > 11) {
    month -= 12
    year++
  }
  let nextDate = new Date(year, month, 1)
  return new Date(nextDate.getTime() - 1000 * 60 * 60 * 24).getDate()
}
```

## 更新对象属性值

在不给目标对象增加新的属性的同时更新目标对象属性值

```js
const updateObj = function (target, source) {
  const keys = Object.keys(target)
  // const merge = Object.assign(target, source) 此方法会直接改变 target 对象
  const merge = { ...target, ...source }
  const temp = {}
  keys.map((item, index) => {
    temp[item] = merge[item]
  })
  return temp
}
```

## 删除数组某一项

index 从 0 开始

```js
const deleteItem = function (index, arr) {
  arr.splice(index, 1)
}
```

## 获取文件类型

```js
//  方法1
const fileType = fileName.substring(fileName.lastIndexOf('.') + 1)
//  方法2
const fileType = fileName.split('.').pop().toLowerCase()
```

## 复制文字

```js
function copy(targetValue) {
  let transfer = document.createElement('input')
  document.body.appendChild(transfer)
  transfer.value = targetValue // 这里表示想要复制的内容
  transfer.focus()
  transfer.select()
  if (document.execCommand('copy')) {
    document.execCommand('copy')
  }
  transfer.blur()
  document.body.removeChild(transfer)
}
```
