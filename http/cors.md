## cors 跨域请求 全称"跨域资源共享"（Cross-origin resource sharing）

- 普通的 AJAX 请求只能同源使用，cors 允许浏览器跨域发出 XMLHttpRequest 请求
- cors 需要浏览器和服务器同时支持
- 整个 cors 通信都由浏览器自动完成，对于开发者来说，CORS 通信与同源的 AJAX 通信
  没有差别
- 实现 CORS 通信的关键是服务器。只要服务器实现了 CORS 接口，就可以跨源通信。

### cors 分为简单请求和非简单请求

请求头里的 <font color="red">**Origin**</font> 表示 <font color="red">**当前浏览
器地址栏访问的域名**</font>  
请求头里的 <font color="red">**Host**</font> 表示 <font color="red">**当前发送的
XMLHttpRequest 请求的域名地址**</font>

![典型的跨域请求头.png](https://s2.loli.net/2022/06/02/mnPrX5HITCiDvFc.png)

如上图所示，地址栏访问的域名地址为https://www.baidu.com（Origin），  
发送请求的域名地址为 ug.baidu.com（Host）  
二者并非同一域名，浏览器自动判断这是一个跨域请求，就自动在头信息之中，添加一个
Origin 字段。

![典型的跨域响应头.png](https://s2.loli.net/2022/06/02/i5HEhf2ub7dI3kv.png)

- 简单请求  
  发出 cors 请求 --》头信息增加 Origin 字段 --》服务器判断 Origin 值是否在许可范
  围

  1. 不在许可范围 --》返回不包含 Access-Control-Allow-Origin 字段的正常 Http 响
     应,浏览器检测到缺失字段，就抛出一个被 XMLHttpRequest 的 onerror 回调函数捕
     获的错误

  2. 在许可范围 --》返回的响应会多出几个头信
     息` Access-Control-Allow-Origin``Access-Control-Allow-Credentials``Access-Control-Expose-Headers `

Access-Control-Allow-Origin：必须值 它的值要么是请求时 Origin 字段的值，要么是一
个\*，表示接受任意域名的请求。  
Access-Control-Allow-Credentials：可选值 表示是否允许发送 Cookie，需前端配合在
AJAX 请求中打开 withCredentials 属性。  
Access-Control-Expose-Headers：可选值 XMLHttpRequest 对象的 getResponseHeader()
可以拿到的其他字段。

- 非简单请求非简单请求会在正式通信前，进行预检请求

![预检请求详情.png](https://s2.loli.net/2022/06/02/rjybS49ZwJsKOQV.png)

除了 Origin 字段，"预检"请求的头信息包括两个特殊字段：

1. Access-Control-Request-Method
2. Access-Control-Request-Headers

服务器响应的 cors 相关字段如下：

1. Access-Control-Allow-Methods
2. Access-Control-Allow-Headers
3. Access-Control-Allow-Credentials
4. Access-Control-Max-Age

![跨域请求.png](https://s2.loli.net/2022/02/17/xF4DmVhKZI9ELaj.png)

![非跨域请求.png](https://s2.loli.net/2022/02/17/TLEhJ1H3fDnP2x6.png)

## 一个典型的图片跨域问题

问题描述：同一张图片，先用 'img' 标签去加载了，然后再在 JS 代码中，创建一个
'img' 并且设置了 crossOrigin 的跨域属性为 'anonymous'，那么在 JS 中创建的 'img'
就会出现访问图片而产生跨域的问题。【服务器必须先配置类似
Access-Control-Allow-Origin: \*这样的跨域权限配置】

```html
<div class="img-wrap">
  <img
    src="https://tenfei05.cfp.cn/creative/vcg/veer/1600water/veer-164825979.jpg"
    alt="通过src加载"
  />
  <img src="https://s2.loli.net/2022/02/17/xF4DmVhKZI9ELaj.png" crossorigin />
  <img src="" class="after-processing" />
</div>
<script>
  let image = undefined;
  let imageDataUrl = undefined;
  // 图片转换为base64格式
  function getBase64Image(imgUrl) {
    image = new Image();
    image.src = imgUrl;
    // 如果不设置crossorigin 则将image传入canvas则会报
    // Tainted canvases may not be exported.(受污染的画布可能无法导出)错误
    image.crossOrigin = "Anonymous";
    image.addEventListener("load", imageReceived, false);
    image.onerror = function (e) {
      console.log(e);
    };
  }
  function imageReceived() {
    let canvas = document.createElement("canvas");
    let ctx = canvas.getContext("2d");
    canvas.width = image.width;
    canvas.height = image.height;
    console.log(image);
    ctx.drawImage(image, 0, 0, image.width, image.height);
    imageDataUrl = canvas.toDataURL("image/png"); // 可选其他值 image/jpeg
    let afterProcessing = document.getElementsByClassName("after-processing");
    afterProcessing[0].src = imageDataUrl;
  }
  getBase64Image("https://s2.loli.net/2022/02/17/xF4DmVhKZI9ELaj.png");
</script>
```
