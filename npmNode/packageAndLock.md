## 先讲一下 package.json 中的版本号

关于版本号的详细信息可查看[semver](https://semver.org/)，这里简要说明一下，例如 npm5.2.1,  
5 是主版本号， 2 是次版本号，1 是 bug 修复版本  
打开 package.json 文件我们会看到类似这样的版本号` "@vant/weapp": "^1.6.4"`

除了 `^`符号，还有`~`符号  
`^`表示执行 `npm install` 时，匹配当前主版本号下的大于当前版本号的最新次版本号，例如：  
**"@vant/weapp": "^1.6.4"表示：匹配安装 ["1.6.4","2.0.0") 之间的最新版本号**  
`~`表示执行 `npm install` 时，匹配当前**主版本 和 次版本** 号下的最新 bug 修复版，例如：  
**"@vant/weapp": "~1.6.4"表示：匹配安装 ["1.6.4"至"1.7.0") 之间的最新版本号**  
无符号则表示安装指定版本号，例如：  
**"@vant/weapp": "1.6.4"表示：安装版本号为"1.6.4"的版本**

这样就会出现一个问题，例如我在本地创建的 xxx 项目，构建环境时，xxx 用的某个依赖 A 的版本号是 4.2.1，"A": "^4.2.1"被写进
package.json 里，过了段时间，我的同事周星星修改我的项目，此时 A 依赖也进行了升级，最新版本号为 4.2.5，则当周星星构建依赖
执行 npm install 时，则会安装 4.2.5，恰好这个更新又会导致 xxx 项目出现问题（理论上 4.2.1 和 4.2.5 应该是兼容的），问题就
出现了。  
package-lock.json 就是用来解决上述问题的。npm V5.xx 以后会自动创建 package-lock.json 文件。

## 再比较 package.json 与 package-lock.json

npm V5.xx 中同时存在 package.json 与 package-lock.json，新的问题又出现了，二者谁的优先级更高，需不需要提交
package-lock.json？ 不说废话，npm 官方几经更新周折之后，最终结果是：

1.  官方建议上传 package-lock.json。
2.  如果 package.json 和 package-lock.json 文件不一致，则执行 npm install 时会根据 package.json 中的版本号和语义安装依赖
    ，并更新至 package-lock.json 。如果二者一致，则执行 npm install 时会根据 package-lock.json 进行安装。

打个比方，  
package-lock.json 是老公  
package.json 是老婆  
老公和老婆意见一致时，听老公的，意见不一致时，听老婆的。

npm init 时只会生成 package.json 文件 npm install 时会生成 package-lock.json 文件

## 参考博文

> [package-lock.json 官方文档](https://docs.npmjs.com/cli/v6/configuring-npm/package-lock-json)  
> [package.json Vs package-lock.json](https://dltlabs.medium.com/package-json-vs-package-lock-json-c8d5deba12cb)  
> [Everything You Wanted To Know About package-lock.json But Were Too Afraid To Ask](https://medium.com/coinmonks/everything-you-wanted-to-know-about-package-lock-json-b81911aa8ab8)
