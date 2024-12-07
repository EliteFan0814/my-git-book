## 常用 npm 命令

- `node app.js` 显式的告诉 shell 使用 node 运行 app.js  
  可以使用 shebang 嵌入要运行的 js 文件的首行来告诉操作系统使用那个解释器运行脚
  本：
  ```javascript
  #!/usr/bin/node
  // 你的代码
  ```
  以上，我们显式地给出了解释器的绝对路径。 并非所有操作系统的 bin 文件夹中都有
  node，但都应该有 env。 您可以告诉操作系统使用 node 为参数运行 env：
  ```javascript
  #!/usr/bin/env node
  // 你的代码
  ```
  要使用 shebang，您的文件应该具有可执行权限。 您可以通过运行以下赋予 app.js 可
  执行权限：
  ```bash
  chmod u+x app.js
  ```
- `npm view 包名@a.b.c` 安装指定版本号为 a.b.c 的包
- `npm view 包名 versions` 列出软件包所有版本号
- `npm view 包名 version` 列出软件包当前最新版本号
- `npm outdated` 列出软件包新版本
- `npm list` 列出安装的软件包  
  `npm list --depth=1` 列出嵌套深度为 1 级的软件包名 列表

```bash
test@0.0.1 C:\Users\xxx\Desktop\testPackage-lock
└┬ cowsay@1.4.0
  ├── get-stdin@5.0.1
  ├── optimist@0.6.1
  ├── string-width@2.1.1
  └── strip-eof@1.0.0
```
