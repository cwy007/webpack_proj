# webpack基础用法

## entry

用来指定打包的入口 - 参考模块依赖图

* 单入口：entry 是一个字符串
* 多入口：entry 是一个对象

```js
entry: './src/index.js'

entry: {
  index: './src/index.js',
  search: './src/search.js'
}
```

## output

用来指定打包的输出 - 告诉 webpack 如何将编译后的文件输出到磁盘

```js
output: {
  filename: '[name].js', // 通过占位符确保文件名称的唯一
  path: __dirname + '/dist'
}
```

## loaders - 解决 webpack 不能识别的文件

webpack 开箱即用只支持 js 和 json 两种文件类型，通过 loaders 去支持其它文件类型并且把它们转化成有效的模块，并且可以添加到依赖图中。

本身是一个函数，接受源文件作为参数，返回转换的结果

### 常见的 loaders 有哪些？

* babel-loader 转换 ES6、ES7 等 js 新特性语法
* css-loader 支持 .css 文件的加载和解析
* less-loader 将 less 文件转换成 css
* ts-loader 将 TS 转换成 js
* file-loader 进行图片、字体等的打包
* raw-loader 将文件以字符串的形式导入
* thread-loader 多进程打包 js 和 css

loaders 的用法

```js
module: {
  rules: [
    {
      test: /\.txt$/, // test 自定匹配规则
      use: 'raw-loader' // use 指定使用的 loader 名称
    }
  ]
}
```

## plugins

插件用于 bundle 文件的优化，资源管理和环境变量注入，作用于整个构建流程

### 常见的 plugins 有哪些？

* CommonChunkPlugin 将 chunks 相同的模块代码提取成公共 js
* CleanWebpackPlugin 清理构建目录
* ExtractTextWebpackPlugin 将 css 从 bundle 文件里提取成一个独立的 css 文件
* CopyWebpackPlugin 将文件或者文件夹拷贝到构建的输出目录
* HtmlWebpackPlugin 创建 html 文件去承载输出的 bundle
* UglifyjsWebpackPlugin 压缩 js
* ZipWebpackPlugin 将打包出的资源生成一个 zip 包

```js
// 放到 plugins 数组中
plugins: [
  new HtmlWebpackPlugin({
    template: './src/index.html'
  })
]
```

## mode -- webpack 4 中提出的概念

指定当前的构建环境是：production、development 还是 none

设置 mode 可以使用 webpack 内置的函数，默认值为 production

### mode 的内置函数功能

* development
  * 设置 process.env.NODE_ENV 的值为 development
  * 开启 NamedChunksPlugin 和 NamedModulesPlugin
* production
  * 设置 process.env.NODE_ENV 的值为 production
  * 开启 FlagDependencyUsagePlugin, FlagIncludedChunksPlugin, ModuleConcatenationPlugin, NoEmitOnErrorsPlugin, OccurrenceOrderPlugin, SideEffectsFlagPlugin 和 TerserPlugin
* none
  * 不开启任何优化选项

## 资源解析

### 解析 ES6

使用 babel-loader，babel 的配置文件是 .babelrc

```js
rules: [
  {
    test: /\.js$/,
    use: 'babel-loader'
  }
]
```

.babelrc

```js
// preset 是多个 plugin 的集合
{
  "presets": [
    "@babel/preset-env" // ES6 的 babel preset
  ],
  "plugins": [
    "@babel/proposal-class-properties"
  ]
}
```
