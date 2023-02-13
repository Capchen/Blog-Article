> 了解Vue的底层架构，是为非web领域提供Vue能力的大前提。Vue核心分为三大块：core，compiler，platform

原文地址：[https://juejin.im/post/5cef749451882530810e0626#heading-8](https://juejin.im/post/5cef749451882530810e0626#heading-8)
## core
core的作用是依据模板生成对应的虚拟节点——vnode，而后通过diff算法更新视图。所以可以根据生成的vnode，变成你想要的任何东西，不仅仅是在web平台的dom节点。
![](https://cdn.nlark.com/yuque/0/2019/png/297368/1560759707817-e6b12cd5-3e82-4495-9846-3f7b5c9e324b.png#height=259&id=BmMCi&originHeight=259&originWidth=789&originalType=binary&ratio=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=789)
### 挂载
在web平台来说，指的是将生成的dom节点，append到指定的元素节点上。我们可以看到在web平台的项目中main.js中的一段代码:

```javascript
new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```
这就是一个挂载操作，使用render方法生成对应的vnode，然后根据vnode生成指定平台（这里是web平台）的元素，然后append到app节点上。
### 指令
vue中的指令区分为编译时处理和运行时处理这两类，像 `v-for` 和 `v-if` 这些是编译时使用，在render生成vnode时就会生效
### VNode
vnode是依据模板生成的虚拟节点，每一个元素对应一个vnode

```javascript
<div class="box" @click="onClick">------------------对应一个vnode
    <p class="content">哈哈</p>-------对应一个vnode
    <TestComps></TestComps>----------自定义组件同样对应一个vnode
    <div></div>-----------------------对应一个vnode
</div>
```

首先是经过vue的complie模块生成render函数，然后执行render函数方法，生成vnode结构。如下：

```javascript
//这里我只列出关键的vnode信息
{
  tag:'div',
  data:{attr:{},staticClass:'box',on:{click:onClick}},
  children:[{
    tag:'p',
    data:{attr:{},staticClass:'content',on:{}},
    children:[{
      tag:'',
      data:{},
      text:'哈哈'
    }]
  },{
    tag:'div',
    data:{attr:{},on:{}},
  },{
    tag:'TestComps',
    data:{
        attr:{},
        hook:{
            init:fn,           
            prepatch:fn,
            insert:fn,
           destroy:fn
        }
     },
  }]  
}

```

根据vnode关系，可以很清晰区分子节点和父节点。
### vm
在vue中vm是组件实例。是对应一个自定义组件的，即只有自定义组件才有vm。
### nextTick
在下一个事件循环执行，老版本的vue实现的方式是通过mutationsObserve监听dom变化来响应的，现在2.5+版本则是在promise不支持的情况下使用messageChannel方式（它输入宏任务macrotask）
### watcher
一个组件对应一个watcher,被观察的对象包括data,props,watch方法中的值，当被观察的对象发生变化的时候，会触发render函数生成新的vnode，然后通过diff算法以最小的代价更新视图。
![](https://cdn.nlark.com/yuque/0/2019/webp/297368/1560765258435-1ea9f026-aede-4a67-ae1e-c40de9771637.webp#height=250&id=C3l8y&originHeight=250&originWidth=780&originalType=binary&ratio=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=780)

> **整个watcher体系的建立过程**：
> 1. 创建组件实例的时候会对data和props进行observer，
> 2. 对传入的props进行浅遍历，重新设定属性的属性描述符get和set，如果props的某个属性值为对象，那么这个对象在父组件是被深度observe过的，所以props是浅遍历
> 3. observer会深度遍历data，对data所包含属性重新定义，即defineReactive，重新设定属性描述符的get和set
> 4. 在mountComponent的时候，会new Wacther，当前watcher实例会被pushTarget，设定为目标watcher，然后执行`vm._update(vm._render(), hydrating)`，执行render函数导致属性的get函数被调用，每个属性会对应一个dep实例，在这个时候，dep实例关联到组件对应的watcher，实现依赖收集，关联后popTarget。
> 5. 如果有子组件，会导致子组件的实例化，重新执行上述步骤
> 


### vnode diff 算法

## complier
complier的作用是负责对模板编译，生成render和staticRender函数,这两个函数会生成vnode，给core使用
> template ==> AST ==> 递归ATS生成render和staticRender ==> VNode
> template => parse(正则匹配,AST) => optimize(标注静态节点) => generate(生成render) => 运行render function => VNode


## platform
> platform模块与具体平台相关，我们可以在这里定义平台相关接口传入runtime和compile，以实现具体平台的定制化，因此为其他平台带来Vue能力，大部分工作在这里。


