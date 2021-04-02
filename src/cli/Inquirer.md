# 【开发脚手架工具】三、requirer.js使用说明

建立与用户之间的命令行交互。

## 安装与起步

```javascript
npm install inquirer

var inquirer = require('inquirer')
```

## inquirer.prompt(questions, answers)

开启提问交互界面。

- 参数一：`Array`类型，问题列表，详细说明见下文
- 参数二：`Object`类型，已经回答的问题结果，默认为`{}`
- 返回一个promise

```javascript
// 基本格式
inquirer
  .prompt([
  	// questions
	])
  .then(answers => {
  	// answers
	})
  .catch(error => {
  	// handle error
	})
```

## questions详细配置

- 基本配置如下

```javascript
inquirer.prompt([
  {
		type: 'input, confirm, list, rawlist, expand, checkbox, password, editor', // 提问类型
    name: 'key', // 存储当前问题回答的变量
    message: '', // 问题的描述
    default: 'defaultVal', // 默认值
    choices: []|(answers) => [], // 列表选项，在某些type下可用，并且包含一个分隔符(separator)
    validate (val, answers) {
      return true|false
    }, // 对回答进行校验
    filter (val) {
      return 'newVal'
    }, // 对回答进行处理
    transformer (val) {}, // 对回答的显示效果进行处理(如：修改字体或背景颜色)，不影响答案内容
    when: Boolean|(answers) => {
      return true|false
    }, // 根据前面问题的回答，判断当前问题是否需要被回答
    pageSize: Number, // 修改某些type类型下的渲染行数
    prefix: '', // 修改message默认前缀
    suffix: '' // 修改message默认后缀
  }
]).then(answers => {})
```

- `default`、 `choices`、 `validate`、 `filter` 、 `when` 设置为函数时，都可以异步调用。可以返回一个`Promise`，或者使用`this.async()`方法来获得一个回调，然后用最终值来调用它

  - 返回一个promise

  ```javascript
  // 示例
  {
    type: "checkbox",
    message: "选择颜色",
    name: "color",
    choices: function (answers) {
      return new Promise((resolve, reject) => {
        // 做一些异步操作...
        // 使用resolve返回结果
        resolve([ "red", "blur", "green" ])
      })
    }
  }
  ```

  - 使用`this.async()`回调方法处理

  ```javascript
  {
    type: "checkbox",
    message: "选择颜色",
    name: "color",
    choices: function (answers) {
      var done = this.async() // 定义回调函数
      // 做一些异步操作...
      setTimeout(() => {
        let res = ["red", "blur", "green", "yellow"]
        done(null, res) // 调用
      }, 2000)
    }
  }
  ```
  
- choices选项

  ```javascript
  {
    type: 'list',
    choices: [
      {
        name: 'display name', // 显示的文字
        short: 'short name', // 选中时显示，name的简写
        value: 'save value', // 选中时保存在answer里的值
        disabled: false, // 选项禁用
        checked: false, // 是否默认选中
        key: 'a single letter' // type为expand时必填，单个单词，小写，且须唯一
      }
    ]
  }
  ```

  

## answers答案

`Object`。包含了每一个提问获得的答案。key为questions对象的name属性值，value为用户的输入结果。

```javascript
{[key]: value}
```



## Separator分隔符

可以在`choices`数组的任意位置加入分隔符

```javascript
choices: [
  "Choice A",
  new inquirer.Separator("--- 自定义分隔符 ---"),
  "choice B"
]
```

## 用户界面和布局

`inquirer.ui.BottomBar`：在一个自由文本区域的底部显示一个固定的文本

```javascript
var ui = new inquirer.ui.BottomBar()

// simply write output
ui.log.write('something just happened.')

// During processing
ui.updateBottomBar('new bottom bar content')
```

## 实时接口

用于动态添加问题等高级使用场景

```javascript
var prompts = new Rx.Subject();
inquirer.prompt(prompts);

// At some point in the future, push new questions
prompts.next({
  /* question... */
});
prompts.next({
  /* question... */
});

// When you're done
prompts.complete()
```

