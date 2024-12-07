### 浏览器 ERR_BLOCKED_BY_CLIENT 错误

错误原因：很可能是浏览器插件导致的，例如广受欢迎的 ADBlock

恰好我今天遇到了 ADBlock 导致的网页不能正常加载的问题，记录说明一下：

![err-blocked-by-client.png](https://s2.loli.net/2022/10/31/dJGXjtnOcykAFKa.png)

原因是请求里的 adsense 字段触发了 ADBlock 黑名单：

![adblock拦截请求.png](https://s2.loli.net/2022/10/31/QpC1LU6kGHr2cXS.png)

解决方式很简单：把当前网站加入白名单就行了。
