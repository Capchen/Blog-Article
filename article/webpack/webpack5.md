## node版本
node版本最低需要`v10.13.0`

## 功能清除

- require.includes语法被废弃 ，这个语法是模块预加载
- 不再为node加载全部的polyfill，如果某个模块依赖了node.js的核心模块，之前的版本会将node的polyfill整个引入，5则不会  

## 长期缓存	
 确定的moduleIds/chunkIds和导出名称
文件没有改变，就不会改变
 
## 持久化缓存 
4里面需要使用cache-loader去做些优化
5里面默认支持，cache字段控制

## Tree-Shaking

- 嵌套的tree-shaking ，嵌套的引入，会将无用的方法摇掉
- 内部模块的tree-shaking，分析内部模块的依赖关系，无用的依赖会被摇掉 

## 代码生成
支持生成ES6模块代码
```javascript
output: {
  ecmaVersion: 6
}
```
## 开创性特性：模块联邦 
概念：一个javascript应用程序可以在运行过程中动态加载另一个应用的代码，并支持共享依赖。不再需要本地加载依赖包

- Remote 被依赖方，被host消费的应用 （提供服务）
- host 依赖方， 消费其他remote的应用（消费）

webpack内部通过` ModuleFederationPlugin`插件实现的将多个模块联合起来
 
