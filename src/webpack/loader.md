# loader

用于对模块的源代码进行预处理和转换。



## loader的使用

- cli：在shell命令中使用

```javascript
// --module-bind
webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
```

- 内联：在引用模块时指定 loader。用`!`分开多个loader、用`?`为loader传递参数

```javascript
import Styles from 'style-loader!css-loader?modules!./styles.css'
```

- 配置：在webpack.config.js中配置loader
  - `推荐使用`：配置统一、维护方便、代码更简洁

```javascript
// 在配置中使用loader的基本格式
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          }
        ]
      }
    ]
  }
}
```



## loader特性

- loader支持链式传递，多个loader执行顺序`从后往前`
- 前一个loader的返回值传递给下一个loader；最后一个loader返回预期结果给webpack
- 可以是同步的或异步的
- 运行在nodeJS中，能够执行任何操作
- 可接收查询参数或通过options配置参数
- loader会产生额外的任意文件



## 常用loader



### 文件

- file-loader：文件处理

```javascript
{
	test: /\.(png|jpg|gif)$/,
  use: [
    {
      loader: 'file-loader',
      options: {
        name: '[name].[hash].[ext]', // 文件名，可拼接目录
        publicPath: '', // 文件发布路径
        outputPath: '', // 文件输出目录
      }
    }
  ]
}
```

- url-loader：类似file-loader，在文件大小（单位 byte）低于指定的限制时，可以返回一个 DataURL

```javascript
{
	test: /\.(png|jpg|gif)$/,
  use: [
    {
      loader: 'url-loader',
      options: {
        limit: 8192, // 低于该限制时，返回 DataURL
        fallback: 'file-loader' // 文件大小高于limit的处理loader，默认file-loader
      }
    }
  ]
}
```



### 样式

- style-loader：将样式写入`<style>`标签，建议与`css-loader`结合使用

```javascript
{
  use: 'style-loader',
  options: {
    hmr: true, // 开启热更新
    sourceMap: true, // 启用/禁用sourcemap
    attributes: {}, // 给style/link元素添加属性
    insert: 'head', // 插入的位置
    esModule: true // 默认生成使用ES模块语法的js模块，在模块串联和tree shaking时会非常有用
  }
}
```

- css-loader：解析`@import` 和 `url()`引用的css样式

```javascript
{
  use: 'style-loader',
  options: {
    url: true, // 启用/禁用url()
    import: true, // 启用/禁用@import
    sourceMap: true, // 启用/禁用sourcemap
    modules: false, // 启用/禁用css模块规范
    esModule: true // 默认生成使用ES模块语法的js模块，在模块串联和tree shaking时会非常有用
  }
}
```

[css模块规范参考](../css/css-modules.md)

