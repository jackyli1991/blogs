# entry

项目构建入口。



## 单个入口

```javascript
module.exports = {
  entry: {
    main: './path/to/my/entry/file.js'
  }
}

// 简写为
module.exports = {
  entry: './path/to/my/entry/file.js'
}
```

#### 数组形式

需要将多个依赖文件一起注入并输出到一个“chunk”时，传入数组会很有用。但在扩展配置时`有失灵活性`

```javascript
module.exports = {
  entry: ['babel-polyfill', './path/to/my/entry/file.js']
}
```



## 多个入口

使用对象语法，指定每个入口。`高扩展性`。常用于：

- 分离第三方库
  - `推荐使用具有更佳vendor分离能力的DllPlugin`
- 多页面应用程序

```javascript
module.exports = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
  }
}
```



## 多页面应用通用打包方案

```javascript
npm i glob -D
```

假设工程页面目录如下：

```javascript
|-src
	|-pages
		|-pageA
			|-index.js
			|-index.html
		|-pageB
			|-index.js
			|-index.html
```

利用glob动态加载所有的页面入口，并生成页面对应的htmlWebpackPlugin插件配置：

```javascript
// 生成所有entry和htmlWebpackPlugins
function generateEntries () {
  const entry = {}
  
  const entryFiles = glob.sync(path.join(__dirname, './src/pages/*/index.js'))
  Object.keys(entryFiles).map(index => {
    const entryFile = entryFiles[index]
    const match = entryFile.match(/src\/pages\/(.*)\/index\.js/)
    const pageName = match && match[1]
    entry[pageName] = entryFile
    htmlWebpackPlugins.push(generateHtmlWebpackPlugin(pageName))
  })
  return {
    entry,
    htmlWebpackPlugins
  }
}

// 每个HTML页面都有一个入口起点：生成页面HtmlWebpackPlugin配置
function generateHtmlWebpackPlugin (pageName) {
  return new HtmlWebpackPlugin({
    template: path.join(__dirname, `src/pages/${pageName}/index.html`),
    filename: `${pageName}.html`,
    chunks: [pageName],
    inject: true,
    minify: {
      html5: true,
      collapseWhitespace: true,
      preserveLineBreaks: false,
      minifyCSS: true,
      minifyJS: true,
      removeComments: false
    }
  })
}
```

