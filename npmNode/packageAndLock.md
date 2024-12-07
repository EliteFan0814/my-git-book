## 先讲一下 package.json 中的版本号

关于版本号的详细信息可查看[semver](https://semver.org/)，这里简要说明一下，例如
npm5.2.1,  
5 是主版本号， 2 是次版本号，1 是 bug 修复版本  
打开 package.json 文件我们会看到类似这样的版本号` "@vant/weapp": "^1.6.4"`

除了 `^`符号，还有`~`符号等

- `^`表示执行 `npm install` 时，匹配当前主版本号下的大于当前版本号的最新次版本号
  ，例如：  
  **"@vant/weapp": "^1.6.4"表示：匹配安装 ["1.6.4","2.0.0") 之间的最新版本号**
- `~`表示执行 `npm install` 时，匹配当前**主版本 和 次版本** 号下的最新 bug 修复
  版，例如：  
  **"@vant/weapp": "~1.6.4"表示：匹配安装 ["1.6.4"至"1.7.0") 之间的最新版本
  号**  
  无符号则表示安装指定版本号，例如：  
  **"@vant/weapp": "1.6.4"表示：安装版本号为"1.6.4"的版本**

- \>: 接受高于指定版本的任何版本。
- \>=: 接受等于或高于指定版本的任何版本。
- <=: 接受等于或低于指定版本的任何版本。
- <: 接受低于指定版本的任何版本。
- =: 接受确切的版本。
- -: 接受一定范围的版本。例如：2.1.0 - 2.6.2。
- ||: 组合集合。例如 < 2.1 || > 2.6。

这样就会出现一个问题，例如我在本地创建的 xxx 项目，构建环境时，xxx 用的某个依赖
A 的版本号是 4.2.1，"A": "^4.2.1"被写进 package.json 里，过了段时间，我的同事周
星星修改我的项目，此时 A 依赖也进行了升级，最新版本号为 4.2.5，则当周星星构建依
赖执行 npm install 时，则会安装 4.2.5，恰好这个更新又会导致 xxx 项目出现问题（理
论上 4.2.1 和 4.2.5 应该是兼容的），问题就出现了。  
package-lock.json 就是用来解决上述问题的。npm v5.xx 以后会自动创建
package-lock.json 文件。

## 再比较 package.json 与 package-lock.json

**以下测试结果基于 npm v8.1.0 版本**

npm v5.xx 中同时存在 package.json 与 package-lock.json，新的问题又出现了，二者谁
的优先级更高，需不需要提交 package-lock.json？ 不说废话，npm 官方几经更新周折之
后，最终结果是：

1.  官方建议上传 package-lock.json。
2.  如果修改了 package.json 中的某些内容：
    - **当 package.json 文件未修改依赖版本号时**，执行 npm install 时会根据
      package-lock.json 中的版本号进行依赖安装。
    - **当 package.json 文件修改了依赖版本号**，保存后，执行 npm install 时，会
      根据 package.json 中的版本号和语义安装依赖，并更新到 package-lock.json 文
      件中。(注意：修改的必须是版本号 A.B.C 本身[即修改了 ABC 中的任何一个]，只
      修改^ 和 ~等符号不会触发更新)

总结：

- 当 package.json 中的 版本号与 package-lock.json 中的版本号不一致时（只匹配版本
  号是否一致，不匹配版本号之前的~或^等符号），执行 npm install 时则以
  package.json 中的语义为准更新或下载依赖，并将结果更新到 package-lock.json。

- 执行 npm update 命令时也会根据 package.json 中的版本号和语义安装依赖，并更新到
  package-lock.json 文件中

npm init 时只会生成 package.json 文件， npm install 时会生成 package-lock.json
文件

## 参考博文

> [package-lock.json 官方文档](https://docs.npmjs.com/cli/v6/configuring-npm/package-lock-json)  
> [package.json Vs package-lock.json](https://dltlabs.medium.com/package-json-vs-package-lock-json-c8d5deba12cb)  
> [Everything You Wanted To Know About package-lock.json But Were Too Afraid To Ask](https://medium.com/coinmonks/everything-you-wanted-to-know-about-package-lock-json-b81911aa8ab8)
