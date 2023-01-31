## 启动
运行命令后，npm会让命令行工具进入，node_module下的bin目录去找是否存在webpack.sh/webpack.cmd,存在就执行，不存在就抛出错误

运行目录：node_modules/webpack/bin/webpack.js	


## loader
loader是一个导出为函数的javaScrpt模块

多个loader串行执行，从右往左

```javascript
module: {
  rules: [
    {
      test: /\.less$/,
      use: [
        'style-loader',
        'css-loader',
        'less-loader'
      ]
    }
  ]
}
```

开发调试：loader-runner


## plugin
基本结构
```javascript
class Myplugin（）{
  apply(compiler) {						------- 插件需要实现apply方法
    compiler.hooks.done.tap('', (
        stats									------- 插件的hooks实现（compiler的hooks和compileration的hooks）
      ) => {
    })
  }
}
module.export = Myplugin;
```

## loader和plugin的区别
loader更多是处理各种静态资源，
插件是伴随整个webpack 过程的，loader没法做的，插件都能做。
插件没有独立的开发环境，loader有loader-runner
