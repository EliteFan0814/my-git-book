## MSBUILD : error MSB4132: 无法识别工具版本“2.0”。可用的工具版本为 "4.0"

win10 64 位系统, nodejs 使用 npm install 出现报
错`MSBUILD : error MSB4132: 无法识别工具版本“2.0”。可用的工具版本为 "4.0"`。

1. **问题原因**：  
   项目依赖中，node-gyp 调用 MSBuild 工具进行组件编译，找不到 MSBuild2.0，而系统里只有 MSBuild4.0。
2. **解决方法**：  
   a. 在项目内重新执行 `npm install --msvs_version 2015` 进行安装项目依赖  
   b. 你也可以执行`npm config set msvs_version 2015 --global`进行全局设置，就不需要每次`npm install`时都指定
   msvs_version 2015

#### 参考博文

> [参考博文 1-页面内搜索 mousetraps ](https://github.com/nodejs/node-gyp/issues/629)

> [参考博文 2](https://www.cnblogs.com/iTlijun/p/8193588.html)

## vite 相关

如下使用`npm create vite@latest`构建项目，在 windows 环境下不能使用上下箭头选择

```shell
fan@DESKTOP-F8A5QC7 MINGW64 /f/projectsMy
$ npm create vite@latest
? Project name: » vite-projectvitedemo
√ Project name: ... vitedemo
? Select a framework: » - Use arrow-keys. Return to submit.
>   vanilla
    vue
    react
    preact
    lit
    svelte

```

改换成`winpty npm.cmd create vite@latest`就可以了

```shell
fan@DESKTOP-F8A5QC7 MINGW64 /f/projectsMy
$ winpty npm.cmd create vite@latest
√ Project name: ... vitedemo
? Select a framework: » - Use arrow-keys. Return to submit.
>   vanilla
    vue
    react
    preact
    lit
    svelte

```

## 老旧项目安装 node-sass 报错

```shell
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! node-sass@4.14.1 postinstall: `node scripts/build.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the node-sass@4.14.1 postinstall script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2024-07-04T06_56_09_803Z-debug.log
```

配置淘宝镜像仓库

```shell
npm config set sass_binary_site https://npmmirror.com/mirrors/node-sass
npm config set registry https://registry.npmmirror.com
```

## 老旧项目安装 chromedriver 报错

报错如下：

```shell
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! chromedriver@2.46.0 install: `node install.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the chromedriver@2.46.0 install script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
```

原因是 chromedriver 的部分文件被国内网络给墙掉了，执行以下命令，解决方法：

```shell
npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver
```

更新：npm.taobao.org 证书已经过期了，上面方法不好使了，我这里换成如下淘宝镜像的新地址依然报错：

```shell
npm install chromedriver --chromedriver_cdnurl=https://npmmirror.com/binaries/chromedriver
```

只能放弃淘宝，我换成了华为镜像,注意，地址最后的【/】不能缺失，否则报【500】错误

```shell
npm install chromedriver --chromedriver_cdnurl=https://repo.huaweicloud.com/chromedriver/
```

可以使用【.npmrc】文件统一配置这种类似的配置,比如：

```shell
# .npmrc 文件
sass_binary_site=https://npmmirror.com/mirrors/node-sass
chromedriver_cdnurl=https://repo.huaweicloud.com/chromedriver/
phantomjs_cdnurl=https://npmmirror.com/mirrors/phantomjs
electron_mirror=https://npmmirror.com/mirrors/electron

registry=https://registry.npmmirror.com/
```
