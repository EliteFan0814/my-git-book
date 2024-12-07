## VUE3 自行构建 sfc 遇到的坑

最近在使用 webpack5+vue3 自行搭建一个单文件组件框架，顺便学习一下 webpack5 的改变和 vue3 的新特性，在这里记录一下遇到的
一些问题。

#### 安装 Vue 相关

**注意**以下使用的是 vue3 进行的构建，所以所有依赖都是支持 vue3 的版本在 vue2 中单文件组件配套工具是
vue-template-compiler，vue3 使用 @vue/compiler-sfc 替换掉了 vue-template-compiler

```sh
# 安装最新的vue-loader
$  npm install vue-loader@next

# 安装最新稳定版vue
$ npm install vue@next

# 安装和vue版本配套一致的单文件组件的配套工具@vue/compiler-sfc
$ npm install -D @vue/compiler-sfc
```
