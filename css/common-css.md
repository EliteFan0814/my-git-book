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
  background-image: url('fpc.png');
  background-repeat: no-repeat;
  background-size: 100% 100%;
}
```

## 绝对定位居中

```css
/*水平居中*/
.horizontal-center {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
/*垂直居中*/
.vertical-center {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
/*水平垂直居中*/
.absolute-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
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
  content: '';
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
