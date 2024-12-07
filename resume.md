### 基于 vue-element-admin 这个开源项目的 vue-admin-template 模板

[vue-element-admin 官方文档](https://panjiachen.github.io/vue-element-admin-site/zh/guide/)

使用的 vue-admin-template 模板的 permission-control 分支

### 主要注意点

#### 权限管理

[官方权限管理说明](https://panjiachen.github.io/vue-element-admin-site/zh/guide/essentials/permission.html)

整体流程：

1.  先登录获取 token
2.  然后在路由前置守卫中使用 getInfo 来获取用户角色
3.  根据角色来动态生成路由
4.  渲染左侧路由列表

![router-beforeEach.png](https://i.loli.net/2021/04/30/gBEIrXvCFmJtfaQ.png)

vue-admin-template 的 角色必须是一个非空的数组， 比如：`['admin'] ['admin','editor','manager']` 咱们自己的项目有些没有角
色管理，有些带角色管理返回的可能只是一个简单的 roleType 字符串，这种情况直接跟后端说，好添加 就添加一个字段，不好添加，
前端根据返回信息自己造个数组就行了。 vue-admin-template 的角色是哦通过 getInfo 获取的，咱们 可能登录的时候就给了，这个我
都在关键部分做了注释，根据情况灵活运用。

![getinfo.png](https://i.loli.net/2021/04/30/o8eKvTwHYkyVU75.png)

#### 分页处理 和 iconfont 的使用

[vue-element-admin 功能文档](https://panjiachen.github.io/vue-element-admin-site/zh/feature/component/svg-icon.html)

#### 弹框编辑和新增

弹框的主要问题就是子组件的生命周期和 props 属性传递出现问题常见的控制子组件显示隐藏的方式有$refs 和 v-if ，这里咱们使 用
v-if 来控制子组件的显示隐藏，这样的一个好处是子组件会有完整的生命周期，会杜绝一些比较恶心的 bug。

props 传值，不要直接修改自组件中的 props 属性值，要在子组件的 data 里重新定义一个属性，将 props 中的值赋值给 data 中对应
的属性。

#### 熟悉 vue.config.js 文档

比 webpack 相比已经简洁了非常多，推荐没事的时候看一遍  
 [vue.config.js](https://cli.vuejs.org/zh/config/)

#### 请求封装

基本上都是基于[axios](https://axios-http.com/zh/docs/intro)进行的二次封装，  
**请一定先读一遍官方文档**  
所有的接口都封装在了`src/api` 文件夹，也建议大家把所有的接口都放在这里，不要在每一个单独页面中使用，后期不好修改维护

#### eslint 的使用

[eslint 文档](https://eslint.bootcss.com/docs/user-guide/getting-started)  
 建议大家最好能了解和使用 eslint，因为他能帮助你规范和优化你自己的代码，减少垃圾代码堆积。
