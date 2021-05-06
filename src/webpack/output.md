# output

如何输出打包结果。只需配置一个【即使有多个入口】。



## 常用配置

```javascript
module.exports = output: {
  filename: '[name].bundle.js',
  path: path.resolve(__dirname, 'dist'),
  chunkFilename: '[id].js',
  publicPath: ''
}
```

- filename：决定每个输出bundle的名称。可通过占位符来动态生成：
  - hash：构建hash
  - chunkhash：chunk 内容的 hash
  - name：模块名称
  - id：模块标识符(module identifier)
  - query：模块的 query

```javascript
// 示例
output: {
  filename: '[name].[chunkhash:12].bundle.js'
}
```

- path：输出目录，`绝对路径`
- chunkFilename：非入口chunk文件的名称
- publicPath：按需加载和静态资源的发布路径
  - 该选项的值是以 runtime(运行时) 或 loader(载入时) 所创建的每个 URL 为前缀。因此，在多数情况下，此选项的值都会以`/`结束。

```javascript
// 示例
publicPath: "https://cdn.example.com/assets/", // CDN（总是 HTTPS 协议）
publicPath: "//cdn.example.com/assets/", // CDN (协议相同)
publicPath: "/assets/", // 相对于服务(server-relative)
publicPath: "assets/", // 相对于 HTML 页面
publicPath: "../assets/", // 相对于 HTML 页面
publicPath: "", // 相对于 HTML 页面（目录相同）
```



## 打包NPM库

```javascript
module.exports = {
  output: {
    library: '',
    libraryTarget: ''
  }
}
```

- library：库的名称

  - 从 webpack 3.1.0 开始，可以指定为对象，用于给每个 libraryTarget 起不同的名称

  ```javascript
  library: {
    root: "MyLibrary",
    amd: "my-amd-library",
    commonjs: "my-common-library"
  }
  ```

  

- libraryTarget：如何暴露库
  - var：暴露为一个变量，赋值给output.library
  - assign：暴露为一个全局变量，可能会覆盖全局中已存在的值【谨慎使用】
  - this：分配给this的一个属性
  - window：分配给window
  - global：分配给global
  - commonjs：分配给exports对象，作为CommonJS环境下使用
  - commonjs2：分配给module.exports对象，作为CommonJS环境下使用
  - amd：暴露为一个AMD模块，供RequireJS 或任何兼容的模块加载器使用
  - umd：将 library 暴露为所有的模块定义下都可运行
  - jsonp：将模块包裹到一个 jsonp包装容器中