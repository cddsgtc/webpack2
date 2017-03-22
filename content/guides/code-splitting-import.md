# 代码分离-使用import()
## 动态导入
目前，类函模`import()`块加载的[`语法建议——syntax proposal`](https://github.com/tc39/proposal-dynamic-import)整体交给ECMAScript。
[`ES2015(es6)加载器说明`](https://whatwg.github.io/loader/)定义`import()`作为一个方法用来动在运行时态加载es6模块。
在webpack中的`import()`是个分离点——split-point，用来把被请求的模块独立成一个单独的模块。`import()`吧模块的名字作为一个参数，并且返回一个`Promise: import(name)->Promise.`

**index.js**
```
function determineDate() {
  import('moment').then(function(moment) {
    console.log(moment().format());
  }).catch(function(err) {
    console.log('Failed to load moment', err);
  });
}

determineDate();
```
## babel配合
如果你想使用babel时使用`import`，你需要使用`syntax-dynamic-import`插件（babel的插件，卸载.babelrc中）,然而该差价仍停留在Stage3（第三阶段）,会出现编译错误。如果建议到了说明推广阶段，那么这个标准见不被采用（指ECMAScript标准演进）。

```
npm install --save-dev babel-core babel-loader babel-plugin-syntax-dynamic-import babel-preset-es2015
# 如下示例，加载moment
npm install --save moment
```
**index-es2015.js**
```
function determineDate() {
  import('moment')
    .then(moment => moment().format('LLLL'))
    .then(str => console.log(str))
    .catch(err => console.log('Failed to load moment', err));
}

determineDate();
```
**webpack.config.js**
```
module.exports = {
  entry: './index-es2015.js',
  output: {
    filename: 'dist.js',
  },
  module: {
    rules: [{
      test: /\.js$/,
      exclude: /(node_modules)/,
      use: [{
        loader: 'babel-loader',
        //如果有这个设置则不用在添加.babelrc
        options: {
          presets: [['es2015', {modules: false}]],
          plugins: ['syntax-dynamic-import']
        }
      }]
    }]
  }
};
```
**注意**
使用`syntax-dynamic-import`插件时，如下情况将报错。

* `Module build failed: SyntaxError: 'import' and 'export' may only appear at the top level`, or (import 和 export只能在最外层，也就是不能用在函数或者块中)
* `Module build failed: SyntaxError: Unexpected token, expected {`

## 配合babel， `async`/`await`

使用ES2017 async/await 配合`import()`:
```
npm install --save-dev babel-plugin-transform-async-to-generator babel-plugin-transform-regenerator babel-plugin-transform-runtime
```
**index-es2017.js**
```
async function determineDate() {
  const moment = await import('moment');
  return moment().format('LLLL');
}

determineDate().then(str => console.log(str));
```
**webpack.config.js**
```
module.exports = {
  entry: './index-es2017.js',
  output: {
    filename: 'dist.js',
  },
  module: {
    rules: [{
      test: /\.js$/,
      exclude: /(node_modules)/,
      use: [{
        loader: 'babel-loader',
        options: {
          presets: [['es2015', {modules: false}]],
          plugins: [
            'syntax-dynamic-import',
            'transform-async-to-generator',
            'transform-regenerator',
            'transform-runtime'
          ]
        }
      }]
    }]
  }
};
```
## `import` 替代 `require.ensure`?

好的方面：使用`import()`能够在加载模块失败时进行错误处理，因为返回的是个`Promise`（基于promise）。

警告：`require.ensure`可以使用参数给模块命名，然而`import`目前上不具备改功能，如果你需要保留该功能很重要，可以继续使用`require.ensure`。
```
require.ensure([], function(require) {
  var foo = require("./module");
}, "custom-chunk-name");
```

## `System.import`被替代
因为在webpack中使用`System.import`已经不合建议规范，因此将在webpack版本v2.1.0-beta.28中启用。建议使用`import()`。

## 例子
* https://github.com/webpack/webpack/tree/master/examples/harmony
* https://github.com/webpack/webpack/tree/master/examples/code-splitting-harmony
* https://github.com/webpack/webpack/tree/master/examples/code-splitting-native-import-context
