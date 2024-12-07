## vscode 中.scss 实时转换为.wxss / vscode 中使用 scss

1. 首先在 vscode 中安装插件 easySass
2. 在 vscode 首选项中找到 settings.json 配置文件
3. 在 settings.json 配置文件中添加如下代码

   ```javascript
   {
   "easysass.formats": [
    {
      "format": "expanded", // 转化样式：没有缩进的、扩展的css代码
      "extension": ".wxss" // sass/scss文件要转化的后缀名
    }
   ]
   }
   ```

4. 同时也支持转化为.css .min.css 等格式，只需再次添加配置就行：

   ```javascript
   {
   "easysass.formats": [
   // ~~~省略~~~
    {
      "format": "expanded",
      "extension": ".css"
    },
    {
      "format": "compressed",
      "extension": ".min.css"
    }
   ]
   }
   ```

5. 顺便在.gitignore 中也可添加忽略 sass 缓存文件的配置

   ```text
   .sass-cache/
   *.css.map
   *.sass.map
   *.scss.map
   *.css
   ```

## 小程序中 px 和 rpx 互相转化

```text
px = rpx / 750 * wx.getSystemInfoSync().windowWidth
rpx = px * 750 / wx.getSystemInfoSync().windowWidth
```

注：  
wx.getSystemInfoSync().windowWidth 单位为 px  
[小程序获取系统信息](https://developers.weixin.qq.com/miniprogram/dev/api/base/system/system-info/wx.getSystemInfoSync.html)

## 顶部安全距离

有时候自定义小程序导航条会遇到刘海/非刘海手机显示不一的问题  
先来看一下微信官方设计稿中的距离：

![胶囊按钮距离](https://i.loli.net/2020/12/07/8Bpvt1iVZ6dHKG3.png)

可以得出：  
胶囊按钮顶部边界距离手机状态栏底部边界的距离是 6px  
胶囊按钮右部边界距离手机屏幕右侧留白安全距离是 6px

```javascript
// 小程序中的 app.js 中定义 capsuleToTop
globalData: {
  //胶囊按钮距离顶部距离= 手机状态栏的高度(px)+6px
  capsuleToTop: wx.getSystemInfoSync()['statusBarHeight'] + 6
}
```

这样在单个 page 文件中就能动态获取胶囊按钮距离手机屏幕顶部的距离了。根据这个距离来自定义小程序导航条：

![胶囊按钮距离](https://i.loli.net/2020/12/07/YlXshuBEreo9FDP.png)

```html
<view class="wrapper" style="padding-top:{{capsuleToTop}}px;"></view>
<!-- 小程序对应的js文件中取出 app.js 中的 capsuleToTop-->
<script>
  Page({
    data: {
      capsuleToTop: app.globalData.capsuleToTop
    }
  })
</script>
```

## 轮播图自适应图片高度

若小程序原生 swiper 组件中的轮播图图片高度不一致，而且还需要宽度占满 750rpx，等比例完全展示，则图片高度就需要使用 js 动
态获取

```html
<swiper
  autoplay="{{true}}"
  interval="{{5000}}"
  duration="{{500}}"
  bindchange="handleChange"
  style="height:{{imgheights[current]}}rpx;"
  class="swiper"
>
  <block wx:for="{{imgUrls}}" wx:key="*this">
    <swiper-item class="swiper-item">
      <view class="block">{{index+1}}/{{imgUrls.length}}</view>
      <image
        data-src="{{item}}"
        data-id="{{index}}"
        mode="widthFix"
        class="swiper-image"
        src="{{item}}"
        bindload="imageLoad"
      />
    </swiper-item>
  </block>
</swiper>
```

```javascript
Page({
  data: {
    imgUrls: [],
    //所有图片的高度
    imgheights: [],
    // 当前 active
    current: 0
  },
  // 处理加载的图片
  imageLoad: function (e) {
    //获取图片真实宽度
    let imgwidth = e.detail.width,
      imgheight = e.detail.height,
      //计算宽高比
      ratio = imgwidth / imgheight
    //计算的高度值
    let viewHeight = 750 / ratio
    let imgheight = viewHeight
    let imgheights = this.data.imgheights
    //把每一张图片的对应的高度记录到数组里
    imgheights[e.target.dataset.id] = imgheight
    this.setData({
      imgheights: imgheights
    })
  },
  handleChange: function (e) {
    this.setData({
      current: e.detail.current
    })
  }
})
```

```css
/* 加入过渡动画 */
.swiper {
  height: 0;
  transition: all 0.3s;
}
```

## 富文本控制图片样式

```html
<style>
  .rich-img {
    width: 100%;
  }
</style>
<script>
  const temp = content.replace(/\<img/gi, '<img class="rich-img" ')
</script>
```

## 小程序复制

```javascript
copyText(e) {
    wx.setClipboardData({
      data: e.currentTarget.dataset.text,
      success: function (res) {
        wx.getClipboardData({
          success: function (res) {
            wx.showToast({
              title: '复制成功'
            })
          }
        })
      },
      fail: function (res) {
        wx.showToast({
          title: '复制失败',
          icon: 'none'
        })
      }
    })
  }
```

## 拨打电话

```javascript
  makeCall(e) {
    const { phone } = app.tapData(e)
    wx.makePhoneCall({
      phoneNumber: phone,
      success() {},
      fail() {}
    })
  },
```

## 隐藏竖向滚动条

```css
::-webkit-scrollbar {
  width: 0;
  height: 0;
  color: transparent;
}
```

## 获取当前页面

```javascript
handleCurrentPage(){
  const page = getCurrentPages().pop()
      if (page == undefined || page == null) return
      // 如果当前页面是 pages/regist/regist
      if (page.route !== 'pages/regist/regist') {
        // to do
      }
}
```

有时从 B 页面做完某个操作返回 A 页面时需要更新 A 页面的某些数据，使用 A 页面中的 onShow 生命周期太 low, 可以使用小程序提
供的路由函数的 success 回掉来处理更新 A 页面数据，例如：

```javascript
wx.navigateBack({
  delta: 1,
  success: function () {
    const page = getCurrentPages().pop()
    if (page == undefined || page == null) return
    page.getUserInfo()
    page.getRewardList()
  }
})
```

## 刷新上个页面数据

```javascript
refreshPrePage(){
  const pages = getCurrentPages()
  const beforePage = pages[pages.length - 2]
  // 对上个页面数据进行操作
  beforePage.refreshData()
}
```
