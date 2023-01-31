### vue中的代码分割（路由懒加载）
结合异步组件和webpack的代码分割功能来实现路由组件的懒加载
> 链接地址 [https://router.vuejs.org/zh/guide/advanced/lazy-loading.html](https://router.vuejs.org/zh/guide/advanced/lazy-loading.html)

首先，将异步组件定义为返回一个Promise的工厂函数
`const foo = () => Promise.resolve({ /*组件定义对象*/ })` 
然后根据webpack代码分割规则，定义分割点
`import('./Foo.vue')`  // 返回Promise

> 注意:
> 动态 `import()` 语法是ECMAScript（JavaScript）[提案](https://github.com/tc39/proposal-dynamic-import)，目前不是语言标准的一部分。预计在不远的将来会被接受。



> 注意
> 如果您使用的是 Babel，你将需要添加 [`syntax-dynamic-import`](https://babeljs.io/docs/plugins/syntax-dynamic-import/) 插件，才能使 Babel 可以正确地解析语法


结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。
```javascript
const Foo = () => import('./Foo.vue')
```

### react中的代码分割
#### 基于路由的代码拆分
```javascript
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import React, { Suspense, lazy } from 'react';
const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));
const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```
上面的代码是路由和lazy配合拆分的示例，和vue的方式大同小异
lazy函数的作用就是允许渲染动态导入为常规组件。
Suspense组件的作用就是类似于骨架屏或者加载指示符，在等待加载的时候出现。

> 注意：
> `React.lazy` 和 Suspense 尚不可用于服务器端渲染。如果要在服务器渲染的应用程序中进行代码拆分，我们建议使用 [Loadable Components](https://github.com/smooth-code/loadable-components) 。它有一个很好的[服务器端渲染打包拆分指南](https://github.com/smooth-code/loadable-components/blob/master/packages/server/README.md)。
> 并且lazy只支持默认导出，并不支持命名导出，想要命名导出那就as defalut

