# 【babel】【前端工程化】关于 babel 的知识整理

## 什么是 babel

下一代 JavaScript 编译器。将使用最新 JS 语法的代码进行转换，以兼容当前运行环境。

## 插件 plugins

babel 只是一个编译器，相当于一个运行框架。实际的代码转换工作需要插件来完成。

> 例如：`@babel/plugin-transform-arrow-functions`插件用于转换 ES6 的箭头函数；`@babel/plugin-transform-for-of`插件用于转换 ES6 的 for···of 循环

- 基本用法

```js
// babel 会自动检查它是否已经被安装到node_modules目录下
plugins: ['babel-plugin-myPlugin']
```

- 短名称：以`babel-plugin-`或`@babel/plugin-`开头的插件可以省略此前缀

```js
plugins: [
  'myPlugin', // 等价于babel-plugin-myPlugin
]
```

- 参数：参数对象和插件名组成一个数组

```js
plugins: [
  [
    'myPlugin',
    {
      // 插件参数
    },
  ],
]
```

## 预设 presets

单个插件具备单一功能。预设是一系列插件的组合。

官方插件包含：

- `@babel/preset-env`：智能预设，`推荐使用，只转译语法，API的转译需要使用垫片`
- @babel/preset-flow：flow 相关
- @babel/preset-react：react 相关
- @babel/preset-typescript：ts 相关
- stage-X：实验性质的预设，分为 5 个提案阶段，`已经废弃`

基本用法（短名称和参数的用法同 plugin 一样）：

```js
presets: [['@babel/preset-env', {}]]
```

## 执行顺序

- plugins 在 presets 前执行
- plugins 执行顺序：顺序执行，从前往后
- presets 执行顺序：颠倒执行，从后往前

## @babel/preset-env 参数

## @babel/polyfill

`babel7.4.0+已废弃使用`，取而代之的，推荐在项目中直接使用用：

```js
import 'core-js/stable'
import 'regenerator-runtime/runtime'
```

### core-js 是什么

core-js 是 JavaScript 标准库的 polyfill，可实现无污染的 API 转译。从源码可以看到，core-js 包含了对 JS 新的 API 的向下兼容处理：

![core-js](https://cdn.jsdelivr.net/gh/jackyli1991/Image-Hosting/img/babel/core-js.png)

core-js 采用模块化；不污染全局环境；并和 babel 高度集成，可对 core-js 的引入优化；

core-js@3 支持最新的稳定功能；增加了对一些 web 标准的支持；支持原型方法并不污染原型；

### regenerator-runtime 是什么

生成器函数、async、await 函数经 babel 编译后，regenerator-runtime 用于提供功能实现。

## @babel/runtime

babel 转译后的代码通常会包含一些辅助函数，这类辅助函数可能会重复的出现在很多模块，导致编译后的代码体积增加。@babel/runtime 的目的就是

## @babel/plugin-transform-runtime
