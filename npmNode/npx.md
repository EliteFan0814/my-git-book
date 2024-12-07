## npx 的使用

**目的：优雅的调用项目内部安装的模块**  

**原理：运行的时候，会到node_modules/.bin路径和环境变量$PATH里面，检查命令是否存在**
```bash
# 未使用npx 项目的根目录下执行
$ node-modules/.bin/mocha --version

# 使用npx 项目的根目录下执行
$ npx mocha --version
```

**避免全局安装的模块**

> 比如：create-react-app这个模块是全局安装的可执行命令，npx 可以运行它，而且不进行全局安装

```bash
# 将 create-react-app下载到一个临时目录，使用以后再删除
$ npx create-react-app my-react-app

# 指定版本
$ npx uglify-js@3.1.0 main.js -o ./dist/main.js

# 强制使用本地模块，不下载远程模块
$ npx --no-install http-server

# 忽略本地的同名模块，强制安装使用远程模块
npx --ignore-existing create-react-app my-react-app

# 指定某个版本的 Node 运行脚本
$ npx node@0.12.8 -v
```

## 参考博文

> [npx 使用教程 阮一峰](http://www.ruanyifeng.com/blog/2019/02/npx.html)  