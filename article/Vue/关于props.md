在编码过程中遇到这样的问题：父组件动态传给子组件一个对象，在子组件里，将这个props赋值给data
代码如下：

```javascript
// 子组件代码
props: ['loanData'],
data () {
  return {
    info: this.loanData,
  }
},
// 父组件代码
<LoanInfo :loanData=loanData></LoanInfo>
// 其中这个loanData初始值为空，通过接口调用来赋值数据
```

按照我的理解：
子组件在父组件的beforeMount生命周期之后开始运行它自己的生命周期函数，由于loanData初始为空，在开始执行子组件的生命周期函数的时候，并不能保证接口已经有了返回值。所以props的值第一次可能为空，也可能有值。
不管props初始有值还是为空，只要父元素的loanData更新了，那么子元素也会得到更新。

以上这些都是没问题，在vue的调试工具中，子组件props总是最新的值，即父组件的loanData更新子组件的props也会更新。

回到上面的代码，在子组件的data时，我是将props初始化了info属性，如果loanData更新，你们觉得info会更新吗？我第一次写的时候就是理所当然的认为会更新，但事实是info并没有改变，在初始化子组件的时候，props.loanData为空，info也就初始化为空，后面就算更新了props.loanData的值，但是data并不会初始化2次。

会出现这样的错误还是对vue的watcher机制不理解。

> 一个组件对应一个watcher，在挂载组件的时候创建这个观察者，组件的state，包含data，props都是被观察者，被观察者的任何变化会被通知到观察者，被观察者的变动导致观察者执行的动作是`vm._update(vm._render(), hydrating)`,组件重新render生成vnode并patch。
> 
> 作者：腾讯IVWEB团队
> 链接：https://juejin.im/post/5cef749451882530810e0626

> 来源：掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


如上描述，子组件的mounted执行完毕是在父组件的mounted生命周期之前，所以也就是先生成子组件的观察者，对于data中info属性和props都建立了观察。接着父元素的data也建立观察。data初始化时loanData为空这个应该没有异议。接着接口返回数据，在父组件中，观测到loanData变化了，那么父组件就回渲染生成新的vnode与旧的vnode进行patch。在这个过程中子组件也会被更新，因为子组件也是对props进行的观察监听**（对传入的props进行浅遍历，重新设定属性的属性描述符get和set，如果props的某个属性值为对象，那么这个对象在父组件是被深度observe过的，所以props是浅遍历）**。所以props.loanData从空到有值。但是为什么info没更新？因为没有触发info的set，也就是没有触发info的更新。wathcer会生效不是自动生效，而是调用了set函数，这个动作才会通知依赖更新。
