---
description: sass文档总结
---

# sass使用总结

## $ 符号声明变量

sass 声明的变量值用作 css 的属性值

```css
/*  声明变量 */
$base-color: #fff;
$base-font: italic arial, sans-serif;
$highlight-color: #f90;

div {
  $base-width: 20px; // 只能在当前 div 中来使用此变量
  color: $base-color;
  font: $base-font;
  width: $base-width;
  border: 1px solid $highlight-color;
}
```

## sass 中中划线和下划线命名通用

```css
/*  $link-color 和 $link_color 通用 */
$link-color: blue;
a {
  color: $link_color;
}
```

## sass 嵌套规则，是个人都能懂，但有注意事项

1. 父选择器的标识符 &  

   sass 解嵌套是把父选择器通过空格连接子选择器，这在 css 中代表后代选择器，但是有时候我们不想要应用到所有后代

```css
/*例如：*/
article a {
  color: blue;
  :hover {
    color: red;
  }
}
/*会被解析成*/
article a {
  color: blue;
}
article a :hover {
  color: red;
}
/*而我们想要的效果是这样的 注意 a 和 :hover 之间无空格*/
article a:hover {
  color: red;
}
/*那就使用 & ，它代表父选择器*/
article a {
  color: blue;
  &:hover {
    color: red;
  }
}
/*会被解析成*/
article a {
  color: blue;
}
article a:hover {
  color: red;
}
```

## @import 导入

sass 的 @import 导入在生成 css 文件时就把相关文件导入进来  
css 的 @important 是只有执行到@import 时，浏览器才会去下载要导入的 css 文件

```css
/*可以省略.sass或.scss文件后缀*/
@import 'colors';
```

1. sass 的局部文件：指并不需要生成对应的独立 css 文件的 sass 文件，sass 有一个特殊的约定来命名这些文件

   以下划线开头,这样，sass 就不会在编译时单独编译这个文件输出 css，而只把这个文件用作导入

2. @import 一个局部文件时,可以省略文件名开头的下划线

```css
/*导入 themes/_night-sky.scss*/
@import 'themes/night-sky';
```

1. @import 可进行局部导入

```css
/*base.scss 文件*/
aside {
  background: blue;
  color: white;
}

/*use.scss 文件*/
.content {
  .parent {
    @import 'base.scss';
  }
}
```

## 优先级

同一个变量出现的越晚优先级越高

```css
// 最终值为 300px
$base-width: 100px;
$base-width: 200px;
$base-width: 300px;
```

```css
/* base.scss 内容*/
$base-width: 100px;

/*use.scss 内容*/
/* $base-width: 200px; 引入 base.scss 前重新给$base-width赋值$base-width最终值还是100px*/
@import 'base';
/* $base-width: 200px; 引入 base.scss 后重新给$base-width赋值，则$base-width最终值是200px*/
.content {
  border: 1px solid red;
  width: $base-width;
}
```

## 默认变量值 !default

同上一个例子，但是加上了 !default

```css
/* base.scss 内容*/
$base-width: 100px !default;

/*use.scss 内容*/
/* base.scss 中的$base-width带有 default 属性，所以在 use.scss 中的任何位置复写$base-width都会取代默认值*/
$base-width: 200px;
@import 'base';
.content {
  border: 1px solid red;
  width: $base-width; /*最终值为200px*/
}
```

## sass 静默注释

/ _····_ / 是 css 的标准注释格式，对其他人直接可见  
// 在 sass 中使用 // 进行注释，其内容不会出现在生成的 css 文件中

## 混合器

大段大段的重用样式的代码，可以通过 sass 的混合器实现大段样式的重用  
@mixin 用来定义一个混合器  
@include 用来插入一个定义好的混合器

```css
/* base.scss 内容*/
$base-width: 100px !default;
@mixin base-block {
  width: 10px;
  height: 10px;
  border: 1px solid green;
}

/*use.scss 内容*/
$base-width: 200px;
@import 'base';
.content {
  border: 1px solid red;
  width: $base-width; /*最终值为200px*/
  @include base-block; /*注意在引用base-block前需要先定义base-block，这里base-block的定义存放在引入的 base.scss中*/
}
```

**混合器和 css 的 class 类 的区别**  
二者很像，都是给一大堆样式命名，最主要的区别是：

1. 类名是应用在 html 中的，是具有语义的  

   混合器则是单纯用在样式表中描述外观的，没有语义

2. 大量的引用混合器会导致生成的 css 代码量过大，影响加载

所以混合器和类名的配合使用，才能写出整洁的 html 和 css

```css
/*只包含规则的混合器*/
@mixin base-block {
  width: 10px;
  height: 10px;
  border: 1px solid green;
}

/*包含属性的混合器*/
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}
```

## 混合器传参

和 javascript 的 function 函数几乎一模一样的用法，可以给参数赋默认值

```css
/* 定义一个混合器 */
@mixin base-color($normal,$hover:red,$visited:#f5f5f5){
  color:$normal;
  &:hover:$normal;
  &:visited:$visited;
}

/* 使用混合器 使用默认值*/
.parent{
  @include base-color
  font-size:15px;
}
/* 使用混合器 使用自定义值*/
.parent{
  @include base-color(black,red,green)
  font-size:15px;
}
/* 使用混合器 参数值不同顺序写法*/
.parent{
  @include base-color($hover:blue,$normal:red,$visited:#f5f5f5)
  font-size:15px;
}
```

