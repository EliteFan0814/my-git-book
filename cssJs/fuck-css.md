---
description: 坑爹的CSS
---

# 坑爹的 CSS

## :nth-child\(an+b\)

这个 CSS 伪类首先找到**所有当前元素的兄弟元素**，然后对**这些所有元素**按照位置先后顺序从 1 开始排序，选择的结果为 CSS 伪类:nth-child 括号中表达式（an+b）匹配到的元素集合（n=0，1，2，3...）  
注意：n 是从 0 开始的，但是通过 an+b 得出的顺序是从 1 开始的：

如下 css：

1. 对当前元素的和当前元素的所有兄弟元素进行排序
2. 同时满足顺序是 an+b 且类名是 course-item 的元素

   才能应用背景色是 f5f5f5 的样式

```css
.course-item:nth-child(2n) {
  background-color: #f5f5f5;
}
/* 这里当n等于0时，如下这个样式是无效的，因为元素要从第1个开始 */
.course-item:nth-child(0) {
  background-color: #f5f5f5;
}
```

再来一个更直观的例子：

![例子](https://i.loli.net/2020/11/24/Z3nCsTNDeUL4XKt.png)

## :first-child 和:last-child

类似于 :nth-child 需满足如下条件：

1. 该元素属于这个类名
2. 该元素是其父元素下的首个或最后一个子元素  
   直观的例子：

![例子](https://i.loli.net/2020/11/24/TkSg9hw43CWbXPv.png)
