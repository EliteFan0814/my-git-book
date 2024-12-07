---
description: canvas 制图
---

## canvas 里的坐标系统

canvas 跟其他标签一样，也可以通过 css 来定义样式。但这里需要注意的是：canvas 的默认宽高为 300px \* 150px,在 css 中为
canvas 定义宽高，实际上是把宽高为 300px \* 150px 的画布进行了拉伸，如果在这样的情况下进行 canvas 绘图，你得到的图形可能
就是变形的效果。所以，在 canvas 绘图时，应该在 canvas 标签里直接定义宽高。如下是一个例子：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>JS Bin</title>
    <style>
      #canvas {
        width: 300px;
        height: 600px;
        border: 1px solid green;
      }
    </style>
  </head>

  <body>
    <canvas id="canvas" width="300" height="300"></canvas>
    <script>
      var c = document.getElementById('canvas')
      var ctx = c.getContext('2d')
      ctx.moveTo(0, 0)
      ctx.lineTo(300, 300)
      ctx.stroke()
    </script>
  </body>
</html>
```

**_注意：在执行绘图的 js 代码时，js 代码中的坐标（`ctx.lineTo(300, 300)`）参考的坐标系统是在标签中直接定义的 width 和
height，而不是在 css 或 style 标签中定义的 width 和 height_**
