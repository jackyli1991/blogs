#  【开发脚手架工具】一、commander使用说明

用于解析用户的命令，实现和用户的交互。

## 起步

```javascript
// install
npm install commander

#!/usr/bin/env node
const { Command } = require('commander')
const program = new Command()
```

## 选项

- 定义选项
  - 参数一：定义选项短名称【-后面接单个字符】和长名称【--后面接一个或多个单词】，使用逗号`、`空格` `或`|`分隔
  - 参数二：选项描述，可省略，用于--help命令时显示
  - 参数三：默认值，可省略

  ```javascript
  // 基本格式
  program.option('-n,--name <items1> [items2]', 'name description', 'defaultvalue')
  ```
  
- 选项类型

  - 布尔类型选项：

  ```javascript
  program
    .option('-d|--debug', 'output debugging')
    .parse()
  
  // 使用，下同
  - mcp -d // { debug: true }
  ```
  
  - 必填参数选项，使用尖括号`< >`：
  
  ```javascript
  program
    .option('-m|--model <modeType>', 'specify a mode type')
    .parse()
  program
    .requiredOption('-m|--model <modelType>', 'specify a mode type', 'default value')
    .parse()
  
  // 必须指定参数
  - mcp -m freedom // { model: 'freedom' }
  ```
  
  requiredOption的区别在于：使用其定义的选项对应的属性字段在解析时一定要有值，不论是默认值，还是从命令行输入。而
  
  
  
  - 可选参数选项，使用方括号`[ ]`：
  
  ```javascript
	program
	  .option('-m|--model [modeType]', 'specify a mode type')
    .parse()
  
  // 不传入参数时值为true，传入参数时值为传入的参数
  - mcp -m // { model: true }
  - mcp -m freedom // { model: 'freedom' }
  ```
  
  - 取反参数选项：以`no-`开头的`boolean`型长选项
  
  ```javascript
  program
    .option('--no-debug', 'not debug')
    .parse()
  
  // 使用--no-xxx
  - mcp --no-debug // { debug: false }
  ```
  
  - 变长参数选项：使用`...`设置可变长参数；用户可以在命令行中输入多个参数，解析后会以数组形式存储在对应属性字段中
  
  ```javaScript
  program
    .option('-n, --number <numbers...>', 'specify numbers')
  	.parse()
  
  // 
  - mcp --number 1 2 3
  ```
  
  - 其他配置选项：使用构造`Option`对象对选项进行更详尽的配置
  
  ```javascript
  const { Command, Option } = require('commander')
  const program = new Command()
  
  // 定义可选值
  program.addOption(new Option('-d, --drink <size>', 'drink size').choices(['small', 'medium', 'large']))
  ```

* 获取选项：
  * 定义完所有选项后，调用`program.parse()`方法解析参数
  * 参数可使用`program.opts()`获取
  * 没有被使用的选项会存放在`program.args`数组中


  ```javascript
  console.log(program.opts())
  ```

## 命令

通过`.command()`或`.addCommand()`可以配置命令。

- `.command()`参数：
  
  - 参数一：配置命令名称及命令参数（支持必选`< >`、可选`[]`及变长参数【只能是最后一个参数】`...`）
  - 参数二：命令描述<可省略>：如果存在，且没有显示调用action(fn)，就会启动子命令程序，否则会报错
- 参数三：配置选项<可省略>：可配置noHelp、isDefault等
  
  ```javascript
  // 格式
  program
    .command('init') // 创建命令
    .alias('i') // 命令简写、别名
    .arguments('<name> [author]') // 设置命令参数
    .option('-s, --save', 'allow save') // 定义选项
    .description('init description', {
      name: 'program name',
      author: 'program author, if needed'
    }) // 命令描述和命令参数描述
    .action((name, options, command) => {
      // 处理函数
    })
```



命令有两种实现方式：为命令绑定`action`处理函数，或者将命令单独写成一个可执行文件。

- action处理函数

  命令声明的所有参数作为处理函数的参数依次传入。同时处理函数还支持两个额外参数：一个是解析出的选项，一个是该命令对象自身

```javascript
program
  .arguments('<name> [addr]')
  .action((name, addr, options, command) => {
    
  })
```

- 独立的可执行文件：

当`.command()`带有描述参数时，意味着使用独立的可执行文件作为子命令。 Commander 将会尝试在入口脚本的目录中搜索`program-command`形式的可执行文件，例如`pm-install`, `pm-search`。通过配置选项`executableFile`可以自定义名字。

```javascript
program
  .command('init <name>', 'init a program name')
	.command('update', 'update installed packages', { executableFile: 'myUpdateSubCommand' })
```

## 其他

### 帮助信息

Commander 基于你的程序自动生成，默认选项是`-h,--help`

```javascript
- mcp --help // 查看帮助信息
- mcp commandName --help // 查看具体子命令的帮助信息

program.addHelpText('after', `
	...
`) // 自定义添加帮助信息
```

通过` .usage 和 .name`修改帮助信息的首行提示：

```javascript
program
  .name("my-command")
  .usage("[global options] command")
```

### .parse()和.parseAsync()

.parse()用于解析输入的命令：

- 第一个参数是要解析的字符串数组，默认是`process.argv`

- 默认遵循node的参数约定，可在第二个参数中传递以下`from`选项：
  - node：默认，`argv[0]`是应用，`argv[1]`是要跑的脚本，后续为用户参数
  - electron：`argv[1]`根据 electron 应用是否打包而变化
  - user：来自用户的所有参数