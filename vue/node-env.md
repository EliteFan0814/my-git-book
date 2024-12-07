## NODE_ENV 的来历

### 1 什么是 process.env

process 对象提供有关当前 Node.js 进程的信息并对其进行控制，它全局可用。  
process.env 属性返回包含用户环境的对象。

vue 项目中常见的 process.env.NODE_ENV 中的 NODE_ENV 是 vue-cli 自己添加的环境变量。

### 2 Vue CLI 中的模式、环境变量、环境文件

一个 Vue CLI 项目有三个模式：

1. development 用于 vue-cli-service serve
2. test 用于 vue-cli-service test:unit
3. production 用于 vue-cli-service build

--mode 选项参数为命令行覆写默认的模式:  
使用开发环境配置构建生产环境应用： `vue-cli-service build --mode development`

环境文件里包含对应模式的环境变量

```
.env                      # 在所有的环境中被载入
.env.local               # 在所有的环境中被载入，但会被 git 忽略
.env.[mode]           # 只在指定的模式中被载入
.env.[mode].local    # 只在指定的模式中被载入，但会被 git 忽略
```

环境文件内容示例：

```
NODE_ENV=development
BASE_URL =www.fpc.com
VUE_APP_NOT_SECRET_CODE=some_value
FOO=bar
```

如果环境文件内部不包含 NODE_ENV 变量，它的值将取决于模式，例如，在 production 模式下被设置为 "production"，在 test 模式
下被设置为 "test"，默认则是 "development"。

假设应用内包含如下若干环境文件：

```
# .env
VUE_APP_TITLE=这是env

# .env.staging
NODE_ENV=production
VUE_APP_TITLE=这是env.staging
```

1. 情况 1：  
   `vue-cli-service build` 会加载可能存在的 .env、.env.production 和 .env.production.local 文件然后构建出生产环境应用。

2. 情况 2：  
   `vue-cli-service build --mode staging` 会在 staging 模式下加载可能存在的 .env、.env.staging 和 .env.staging.local 文
   件然后构建出生产环境应用。

根据 NODE_ENV，两种情况构建出的应用都是生产环境应用，但是 process.env.VUE_APP_TITLE 值不一样。

**环境变量会随着构建打包嵌入到输出代码，任何人都有机会找到这些值，所以不要在其中存储任何机密信息！！！这些环境变量中，
NODE_ENV，BASE_URL 和以 VUE_APP 开头的变量将通过 webpack.DefinePlugin 静态地嵌入到客户端侧的代码中。对于这些环境变量你可
以在应用的代码中直接访问它们：`console.log(process.env.VUE_APP_SECRET)`**
