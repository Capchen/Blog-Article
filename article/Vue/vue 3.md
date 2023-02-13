# VUE 3

## tree-shaking

## fragments碎片

## vite

基于网络请求的依赖加载

## composition API

- 2.X 基于选项的api（options API）
- 3.X 基于函数的api
-- 类型支持
-- 压缩友好

现有api不影响，
共存，组件的封装和架构会更友好

单文件组件太大，不容易切分

## 动机

### 逻辑复用模式

在vue2中（options api）：

- 当一个组件中使用了多个mixin的时候光看模版很难分清一个属性来自哪一个
- 命名空间冲突，多个开发者无法保证属性或者方法名不重复
- 性能方面，高阶组件和无状态组件都需要额外创建组件实例来封装逻辑

## now

composition api 组合式api
composition function 组合函数
function-based api 基于函数的api，使用基于函数的api将相关联的代码抽取到一个组合函数中，该函数封装了相关联的逻辑，并将需要暴露给组件的状态以响应式的数据源方式返回出来。
e.g

```
function useMouse() {
  const x = ref(0)
  const y = ref(0)
  const update = e => {
    x.value = e.pageX
    y.value = e.pageY
  }
  onMounted(() => {
    window.addEventListener('mousemove', update)
  })
  onUnmounted(() => {
    window.removeEventListener('mousemove', update)
  })
  return { x, y }
}

// 在组件中使用该函数
const Component = {
  setup() {
    const { x, y } = useMouse()
    // 与其它函数配合使用
    const { z } = useOtherLogic()
    return { x, y, z }
  },
  template: `<div>{{ x }} {{ y }} {{ z }}</div>`
}
```

> 组合函数useMouse里面就有很多基于函数的api，然后将状态x，y返回出来，组件里面就可以用了


## setup函数

这一个新引入的组件选项，它会在一个组件实例被创建时，初始化了props之后调用。会将接受到的props作为参数

## ref函数

如果我们想在setup内部创建一个被管理的值。可以使用ref函数，它返回的是一个包装对象。
包装函数包装str和num这些基本类型是为了能更好追踪它们的变化。如果一个函数返回的是1这个数字，接收到这个数值的代码只会获得一个值。
包装函数去包装对象或是数组也是有意义的。他可以让我们替换整个对象的值
e.g

```
const numbers = ref([1, 2, 3])
// 替代原数组，但引用不变
numbers.value = numbers.value.filter(n => n > 1)
```

> 当包装对象被暴露给模版渲染上下文，或是被嵌套在另一个响应式对象中的时候，它会被自动展开 (unwrap) 为内部的值。


## Watchers

和 2.x 的 $watch 有所不同的是，watch() 的回调会在创建时就执行一次。这有点类似 2.x watcher 的 immediate: true 选项，但有一个重要的不同：默认情况下 watch() 的回调总是会在当前的 renderer flush 之后才被调用 —— 换句话说，watch()的回调在触发时，DOM 总是会在一个已经被更新过的状态下。 这个行为是可以通过选项来定制的。

> 在 2.x 的代码中，我们经常会遇到同一份逻辑需要在 mounted 和一个 watcher 的回调中执行（比如根据当前的 id 抓取数据），3.0 的 watch() 默认行为可以直接表达这样的需求。


观察多个数据源的时候只要任意一个数据源改变都会出发回调

### 停止观察

`watch()`返回的是一个停止观察的函数

### 清理副作用

watch的回调函数支持传入第三个参数。这第三个参数是一个用来注册清理操作的函数。调用这个函数会注册一个清理函数。

> 什么情况下会用到清理函数？比如当一个异步操作还没没完成，观测的数据就变化了。我们可能要撤销还在等待结果的前一个操作。


#### 和react对比：

react的`useEffect`方法是直接返回一个清理函数。但是vue没有，而是采用传入注册函数来注册清理函数，是因为vue的watch回调中往往是异步操作，而异步的async函数返回的是个promise，这无法保证需要一个立刻注册的清理函数。

#### Watcher 回调的调用时机

默认情况下，所有的 watcher 回调都会在当前的 renderer flush 之后被调用。这确保了在回调中 DOM 永远都已经被更新完毕。如果你想要让回调在 DOM 更新之前或是被同步触发，可以使用 flush 选项：

```
watch(
  () => count.value + 1,
  () => console.log(`count changed`),
  {
    flush: 'post', // default, fire after renderer flush
    flush: 'pre', // fire right before renderer flush
    flush: 'sync' // fire synchronously
  }
)
```

## 强调点

Vue 的 setup() 每个组件实例只会在初始化时调用一次 ，状态通过引用储存在 setup() 的闭包内。
