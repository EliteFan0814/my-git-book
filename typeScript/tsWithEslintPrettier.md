### 在 VScode 中使用 typeScript ESlint prettier

请先在 vscode 插件市场搜索安装 eslint prettier 插件

- vscode 中的 eslint prettier 插件是用来在编辑器中实时显示警告和错误信息的
- 而项目中使用 npm/yarn 安装的 eslint prettier 则是配置文件或执行 lint 的命令行
  工具

#### 项目依赖说明

| 依赖                             | 说明                                                               |
| :------------------------------- | :----------------------------------------------------------------- |
| typescript                       | js 超集，带来类型检查功能                                          |
| eslint                           | 代码静态分析工具，分析代码中的语法错误                             |
| @typescript-eslint/parser        | eslint 的 typescript 解析器,用来识别 typescript 语法               |
| @typescript-eslint/eslint-plugin | eslint 默认规则的补充，提供了一些额外的适用于 ts 语法的规则        |
| prettier                         | 代码格式的检查工具，优化代码格式                                   |
| eslint-plugin-prettier           | 将 Prettier 作为 ESLint 规则运行，并将差异报告为单个 ESLint 问题。 |
| eslint-config-prettier           | 关闭 eslint 中所有不必要或可能与 Prettier 冲突的规则               |

1. 执行 `npm install -D eslint`
   - 手动创建.eslintrc.js 配置文件，
   - 也可以通过命令行`npm init @eslint/config`生成你想要的配置  
     这是我通过`npm init @eslint/config`生成的.eslintrc.js：
   ```javascript
   module.exports = {
     env: {
       browser: true,
       es2021: true,
       node: true,
     },
     extends: ["eslint:recommended", "plugin:@typescript-eslint/recommended"],
     overrides: [],
     parserOptions: {
       ecmaVersion: "latest",
       sourceType: "module",
     },
     rules: {},
   };
   ```
2. 执
   行`npm install -D typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin`
   - .eslintrc.js 中配置"parser"："@typescript-eslint/parser"
   - .eslintrc.js 中配置"plugins"：添加@typescript-eslint
3. 执行 `npm install -D prettier eslint-plugin-prettier eslint-config-prettier`
   - .eslintrc.js 中配置"extends"：添加 plugin:prettier/recommended 到**最后一
     个**
4. 在.eslintrc.js "rules"配置中配置 prettier 相关的检测规则：

   ```javascript
   // 例如这样
     rules: {
       // 配置ts环境下的eslint规则
       "@typescript-eslint/no-var-requires": "warn",
       /*
        如下是在eslint中配置prettier相关规则
        注意，这里配置的是 eslint 要按照什么样的prettier样式来检查代码

         比如下面配置的意思就是：
         如果不按照
        {
           singleQuote: false,
           tabs: false,
           endOfLine: "auto",
           trailingComma: "es5",
           proseWrap: "always",
         }
         这个规则显示的代码就以警告处理

         而具体怎么格式化代码还需单独配置到 prettierrc.json 文件中
       */
       "prettier/prettier": [
         "warn",
         {
           singleQuote: false,
           tabs: false,
           endOfLine: "auto",
           trailingComma: "es5",
           proseWrap: "always",
         },
       ],
     },
   ```

5. 最终配置：

   ```javascript
   module.exports = {
     env: {
       browser: true,
       es2021: true,
       node: true,
     },
     extends: [
       "eslint:recommended",
       "plugin:@typescript-eslint/recommended",
       "plugin:prettier/recommended",
     ],
     overrides: [],
     parser: "@typescript-eslint/parser",
     parserOptions: {
       ecmaVersion: "latest",
       sourceType: "module",
     },
     plugins: ["@typescript-eslint"],
     rules: {
       "prettier/prettier": [
         "warn",
         {
           singleQuote: false,
           tabs: false,
           endOfLine: "auto",
           trailingComma: "es5",
           proseWrap: "always",
         },
       ],
     },
   };
   ```

最需要注意的是将 plugin:prettier/recommended 放在 extends 的最后一个！  
最需要注意的是将 plugin:prettier/recommended 放在 extends 的最后一个！  
最需要注意的是将 plugin:prettier/recommended 放在 extends 的最后一个！

6. 开启保存时使用 eslint 自动格式化功能：  
   在 vscode 的设置配置文件中配置如下选项：

   ```javascripton
     "editor.codeActionsOnSave": {
       "source.fixAll.eslint": true
     },
   ```

#### 参考博文

> [typescript-eslint](https://typescript-eslint.io/docs/)  
> [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)  
> [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier)
