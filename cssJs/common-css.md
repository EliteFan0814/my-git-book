---
description: CSS常用样式集合
---

## 单行/多行省略：

```css
/*单行省略*/
.omit-1 {
  white-space: nowrap;
  text-overflow: ellipsis;
  -o-text-overflow: ellipsis;
  overflow: hidden;
}
/*多行省略*/
.omit-3 {
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
}
```

## 背景图片：

```css
.bg {
  background-color: #fff;
  background-image: url("fpc.png");
  background-repeat: no-repeat;
  background-size: 100% 100%;
}
```

## 绝对定位居中

```css
/*水平居中1*/
/*此方法的问题：定位元素的默认可分配的宽度只有50%的父positioned元素宽度*/
.horizontal-center {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
/*水平居中2*/
.horizontal-center {
  position: absolute;
  left: 1rem;
  right: 1rem;
  width: fit-content;
  margin-inline: auto;
}
/*垂直居中*/
.vertical-center {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
/*水平垂直居中1*/
.absolute-center {
  width: 600px;
  height: 400px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
/*水平垂直居中2*/
.absolute-center {
  width: 600px;
  height: 400px;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
```

## 去除 input 默认样式\(包括 IOS 中 input 的默认阴影\)

```css
input {
  box-shadow: none; /*去除阴影*/
  outline: none; /*聚焦input的蓝色边框*/
  resize: none; /*textarea 禁止拖拽*/
  border: none; /*去除边框*/
  -webkit-appearance: none; /*常用于IOS下移除原生样式*/
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0); /*点击高亮的颜色*/
}
```

## 取消父子元素的透明度传递

父元素使用 rgba 就可以避免：

```css
.parent {
  background: rgba(216, 241, 250, 0.82);
}
.child {
  background: red;
}
```

## 单行文字两边对齐\(兼容 ios\)

\*这里注意左边的 key 值是否带分号，分号也算一个字符！  
html 代码如下：

```markup
  <div class="item-wrap">
    <div class="left-name">
      <div class="name">姓名</div>：
    </div>
    <div class="right-value">
      武大郎
    </div>
  </div>
  <div class="item-wrap">
    <div class="left-name">
      <div class="name">所在城市</div>：
    </div>
    <div class="right-value">
      李家窝棚
    </div>
  </div>
```

css 代码：

```css
.item-wrap,
.left-name {
  display: flex;
  font-size: 24px;
  line-height: 30px;
}
.left-name .name {
  width: 100px; /* 这里一定要使用 width 不能使用 min-width max-width */
  height: 30px; /* 一定要限定高度，高度和 line-height 一致时为上下居中显示 */
  text-align: justify;
  text-align-last: justify;
  text-justify: inter-ideograph;
}
.left-name .name::after {
  display: inline-block;
  content: "";
  width: 100%;
}
```

## 隐藏滚动条

```scss
div {
  width: 24rem;
  height: 20rem;
  overflow: scroll;
  &::-webkit-scrollbar {
    display: none;
  }
  /* IE10+ */
  -ms-overflow-style: none;
  /* firefox */
  overflow: -moz-scrollbars-none;
}
```

## 固定底部无限加载

```css
div {
  box-sizing: border-box;
  width: 100%;
  position: fixed;
  top: 10rem;
  bottom: 0;
  overflow-y: scroll;
}
```

## 固定 div/图片宽高比

有时候需要固定 div 或者图片的宽高比例  
块级元素（如 div,p）的 padding 设置为百分比的时候，是按照父元素的宽度来定的，  
可以利用这一特性，使用 padding-top/padding-bottom 来设置容器高度

![代码示例图片](https://i.loli.net/2020/12/15/Dt1sCIWx4wV9Zzn.png)

以下为代码：

```html
<div class="wrap">
  <div class="img-wrap">
    <!-- 使用 4:9 的图片填充4:3的包裹器 -->
    <img src="https://dummyimage.com/400x900/000/fff" alt="" />
  </div>
  <div class="img-wrap">
    <!-- 使用 4:3 的图片填充4:3的包裹器 -->
    <img src="https://dummyimage.com/400x300/000/fff" alt="" />
  </div>
</div>
```

```scss
.wrap {
  border: 1px solid green;
  .img-wrap {
    border: 1px solid red;
    position: relative;
    margin: 0 20px;
    /*宽度为 父元素总长 - 40px*/
    width: calc(100% - 40px);
    height: 0;
    /*padding-bottom为 (父元素总长 - 40px) * 0.75 也就是宽高比为 4:3*/
    padding-bottom: calc((100% - 40px) * 0.75);
    overflow: hidden;
    img {
      /*开启定位让 img 以 4:3 的的比例填充 img-wrap 包裹器（可能会引起图片变形）*/
      position: absolute;
      width: 100%;
      height: 100%;
    }
  }
}
```

## 1：1 显示不同比例图片

![1v1.png](https://i.loli.net/2021/03/31/bdrQ9xX71yfH5vI.png)

```html
<div class="img-wrap">
  <img src="https://img2.baidu.com/it/u=1612138888,1794405442&fm=26&fmt=auto&gp=0.jpg" alt="" />
</div>
```

```scss
.img-wrap {
  position: relative;
  width: 50%;
  height: 0;
  padding-bottom: 50%;
  img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
}
```

## 不同比例图标的统一显示

有时候界面会用循环来做一个点击列表，列表每一项一般会有小图标，但是每一项的图标比例可能并不一致，这时候就需要让它们根据自
身比例动态调节。  
如下示例图，不同尺寸的 icon 都会按自身比例显示在同样大小的红框中而不会变形：

![示例图](https://i.loli.net/2021/01/08/rsxz4Y5QlB3Wpgn.png)

```html
<div class="img-wrap">
  <img src="https://dummyimage.com/300x400/000/fff&text=icon1" alt="" />
</div>
<div class="img-wrap">
  <img src="https://dummyimage.com/500x300/000/fff&text=icon2" alt="" />
</div>
<div class="img-wrap">
  <img src="https://dummyimage.com/600x200/000/fff&text=icon3" alt="" />
</div>
<div class="img-wrap">
  <img src="https://dummyimage.com/700x500/000/fff&text=icon4" alt="" />
</div>
```

```css
.img-wrap {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 150px;
  height: 150px;
  border: 1px solid red;
}
img {
  max-width: 150px;
  max-height: 150px;
}
```

## 彩色文字

```css
.parent {
  background-image: -webkit-linear-gradient(left, #4af175, #4fe3c0, #51dbee);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

## 带 css 向右三角形的按钮

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>JS Bin</title>
    <style>
      .btn-more {
        background-color: #7d3f98;
        border: none;
        color: #fff;
        display: flex;
        align-items: center;
        padding: 8px 25px;
        text-align: center;
        border-radius: 30px;
        font-size: 16px;
        position: relative;
        cursor: pointer;
      }
      .btn-more:hover,
      .btn-more:active,
      .btn-more:focus {
        background-color: #511378;
      }
      .btn-more:after {
        content: "";
        width: 0;
        height: 0;
        border-style: solid;
        border-width: 6px 0 6px 8px;
        border-color: transparent transparent transparent #fff;
        position: absolute;
        right: 12px;
        top: 50%;
        transform: translateY(-50%);
      }
    </style>
  </head>

  <body>
    <button class="btn-more">more</button>
  </body>
</html>
```

## 带 css 三角形的提示框

```scss
// <div class="tips">请先阅读并同意协议</div>
.tips {
  font-size: 12px;
  white-space: nowrap;
  position: absolute;
  padding: 5px;
  border: 1px solid red;
  background-color: #fff;

  &:before,
  &:after {
    border: solid transparent;
    content: "";
    width: 0;
    height: 0;
    position: absolute;
  }
  &:after {
    border-width: 7px 7px 0 7px;
    border-top-color: #fff; /*浅色*/
    bottom: -7px;
    left: 50%;
    transform: translateX(-50%);
  }
  &:before {
    border-width: 8px 8px 0 8px;
    border-top-color: red; /*深色*/
    position: absolute;
    bottom: -8px;
    left: 50%;
    transform: translateX(-50%);
  }
}
```

## 蒙层禁止页面滚动

在蒙层出现的时候滚动页面，如果不加处理，蒙层底部的页面会开始滚动，实际上我们是不希望他进行滚动的，因此需要阻止这种行为。
最简单的方法： **打开蒙层时给 html/body 添加 overflow: hidden;在关闭蒙层时就移除这个样式**

```js
// 弹窗蒙层打开时设置：
document.documentElement.style.overflow = "auto";
// 弹窗蒙层关闭时设置：
document.documentElement.style.overflow = "hidden";
```

## flex-grow:1 滚动条失效问题

有时候我们需要让在`flex-grow:1`的元素 A 内的元素 B 超出 A 的范围时出现滚动条，但此时在 A 上设置`overflow-y:auto`不管用，
还是会超出 A 的范围，那么就给 A 加上`height:0`这个 css 就能解决超出不出现滚动条的问题。
