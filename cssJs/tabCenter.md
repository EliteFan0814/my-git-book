## 水平滚动 tabs 居中

能够左右滑动的 tabs 标签页，有时候需要将处于 active 状态的 tab 居中显示，效果如下

![tab居中效果图](https://i.loli.net/2021/04/28/A3UyfzgYe29BRap.png)
[在线查看](https://jsbin.com/lopiyutuxa/16/edit?html,css,js,output)  
html 文件如下：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>JS Bin</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>

  <body>
    <div id="app">
      <div class="slide-wrap" id="parent">
        <div
          v-for="(item,index) in tabList"
          @focus="changeTab(index)"
          :key="index"
          :id="'item'+index"
          tabindex="0"
          :class="{item:true,'focus':activeIndex===index?true:false}"
        >
          {{item.name}}
        </div>
      </div>
    </div>
  </body>
</html>
```

CSS 文件如下：

```css
.slide-wrap {
  overflow-x: scroll;
  display: flex;
}
.slide-wrap::-webkit-scrollbar {
  display: none;
}
.item {
  text-align: center;
  white-space: nowrap;
  width: 20%;
  flex-shrink: 0;
  outline: none;
}
.focus {
  box-sizing: border-box;
  transition: border 0.4s;
  box-shadow: none;
  outline: none;
}
.focus:focus {
  outline: none;
  background-color: #ccc;
}
```

JS 文件如下：

```javascript
var app = new Vue({
  el: '#app',
  data: {
    activeIndex: 0,
    tabList: [
      {
        name: '分类0',
        value: 0
      },
      {
        name: '分类1',
        value: 1
      },
      {
        name: '分类2',
        value: 2
      },
      {
        name: '分类3',
        value: 3
      },
      {
        name: '分类4',
        value: 4
      },
      {
        name: '分类5',
        value: 5
      },
      {
        name: '分类6',
        value: 6
      },
      {
        name: '分类7',
        value: 7
      },
      {
        name: '分类8',
        value: 8
      },
      {
        name: '分类9',
        value: 9
      },
      {
        name: '分类10',
        value: 10
      }
    ]
  },
  methods: {
    changeTab(index) {
      this.activeIndex = index
      const father_dom = document.getElementById('parent')
      const son_dom = document.getElementById('item' + index)
      const clientWidth = father_dom.clientWidth
      const offsetLeft = son_dom.offsetLeft
      father_dom.scrollLeft = offsetLeft - clientWidth / 2 + son_dom.clientWidth / 2
    }
  }
})
```
