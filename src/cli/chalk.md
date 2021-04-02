# 【开发脚手架工具】二、chalk使用说明

修改/美化控制台的输出内容的样式。

##  安装与起步

```javascript
npm install chalk

const chalk = require('chalk');
```

## 基本用法

- 样式相关

```javascript
chalk.bold('text') // 加粗
chalk.italic('text') // 倾斜
chalk.underline('text') // 下划线
chalk.hidden('text') // 打印出来并隐藏
chalk.dim('text') // 淡化
chalk.inverse('text') // 反转背景和前景色
```

- 颜色相关

```javascript
chalk.red('text') // 红色
chalk.redBright('text') // 明亮红色
chalk.rgb(24, 123, 45)('text') // 自定义颜色
chalk.keyword('orange')('text') // 橙色
```

- 背景相关

```javascript
chalk.bgRed('text') // 背景红色
chalk.bgRedBright('text') // 背景明亮红色
chalk.bgRgb(15, 100, 204)('Hello!') // 自定义背景色
```

## 模版用法

以`{}`包裹的会被chalk解析，格式为`{chalk方法 要处理的内容}`：

```javascript
console.log(chalk`
	I am a {green.bold 绿色加粗的字体}
`)

// 与js模版字符串搭配使用示例
const miles = 18
const calculateFeet = miles => miles * 5280

console.log(chalk`
	There are {bold 5280 feet} in a mile.
	In {bold ${miles} miles}, there are {green.bold ${calculateFeet(miles)}}.
`)
```

## 链式调用

```javascript
chalk.red.bold('text') // 红色加粗
chalk.bgRgb(15, 100, 204).inverse.underline('Hello!') // 自定义背景色反转+下划线
```

