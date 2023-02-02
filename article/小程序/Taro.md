> 希望能够使用 React 语法写小程序的同时，通过「**Write once Run anywhere**」来实现跨端的。

##### [《小程序跨框架开发的探索与实践》演讲全文]()
### 选型参考
#### 一、
> ![image.png](https://cdn.nlark.com/yuque/0/2022/png/297368/1658458212376-b49929b8-a365-470c-9fa1-0acd1ed4cc25.png#clientId=u96ba7115-3cc6-4&from=paste&height=451&id=ud0ab9d1a&name=image.png&originHeight=444&originWidth=676&originalType=binary&ratio=1&rotation=0&showTitle=false&size=82176&status=done&style=none&taskId=u92fe5621-509a-4378-a80e-93634f9a3ed&title=&width=687)

#### 二、
![image.png](https://cdn.nlark.com/yuque/0/2022/png/297368/1658458298552-b9a62512-3add-48d3-b8ce-977e0a13b8ce.png#clientId=u96ba7115-3cc6-4&from=paste&height=543&id=u3fa7bf97&name=image.png&originHeight=602&originWidth=758&originalType=binary&ratio=1&rotation=0&showTitle=false&size=261352&status=done&style=none&taskId=u65aaf04a-a0b7-41ef-97a0-261314bcd29&title=&width=684)
### 架构
#### 3之前的：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/297368/1653530590832-13699126-4892-4797-abcb-d1739b0b407e.png#clientId=u69f0cedc-3d9c-4&from=paste&height=259&id=u5da2f416&name=image.png&originHeight=448&originWidth=796&originalType=binary&ratio=1&rotation=0&showTitle=false&size=95817&status=done&style=none&taskId=u19591826-32d3-4f6c-bd0c-fc1eb0286e6&title=&width=461)

#### 3之后的
![image.png](https://cdn.nlark.com/yuque/0/2022/png/297368/1658452800658-988fa750-2f13-4a19-855d-ce5c51b4cc75.png#clientId=u96ba7115-3cc6-4&from=paste&height=159&id=udb40ae85&name=image.png&originHeight=422&originWidth=1234&originalType=binary&ratio=1&rotation=0&showTitle=false&size=165055&status=done&style=none&taskId=u9678a04f-a053-4463-97f4-581ad1a7e73&title=&width=465)
##### 区别：
重编译和重运行时，而重运行时带来的好处是，模拟DOM/BOM API：
### ![image.png](https://cdn.nlark.com/yuque/0/2022/png/297368/1653536397477-589a3fed-bdab-47af-87ee-48d025fe255e.png#clientId=u69f0cedc-3d9c-4&from=paste&height=247&id=u87ed2785&name=image.png&originHeight=494&originWidth=1370&originalType=binary&ratio=1&rotation=0&showTitle=false&size=159837&status=done&style=none&taskId=uc60dad64-64b8-42cc-b01b-3454106d5d0&title=&width=685)
### mpvue
mpvue 本质上还是将 Vue 运行在了小程序，在platform目录下实现了小程序的转化。
### Vue支持

- 从`v3.0.0`版本（01 Jul 2020）支持的vue3开发

### Taro小程序

1. 先基于cli为react/vue代码用webpack打包
2. 运行时使用适配器进行适配，调用Taro实现的DOM/BOM API，
3. 渲染到小程序页面

### Taro DOM Tree 如何更新到页面
#### React

1. 先将小程序的组件模版化
2. 将Taro Runtime 生成的Taro Dom Tree，去匹配模版
3. 匹配遍历完Taro Tree之后，得到的就是小程序的组件Tree

![image.png](https://cdn.nlark.com/yuque/0/2022/png/297368/1658458821752-0bb51c59-5d61-4998-8f2a-929d74689e44.png#clientId=u96ba7115-3cc6-4&from=paste&height=274&id=uabb70561&name=image.png&originHeight=359&originWidth=696&originalType=binary&ratio=1&rotation=0&showTitle=false&size=270486&status=done&style=none&taskId=u6e7485aa-b053-46f7-a46c-7c7856e76f7&title=&width=531)

#### Vue
Vue 和 React 最大的区别就在于运行时的 CreateVuePage 方法，这个方法里进行了一些运行时的处理，比如：生命周期的对齐。其他的部分，如通过 BOM/DOM 方法构建、修改 DOM Tree 及渲染原理，都是和 React 一致的。

#### 更新
无论是 React 还是 Vue ，最终都会调用 Taro DOM 方法，如：appendChild、insertChild 等。
这些方法在修改 Taro DOM Tree 的同时，还会调用 enqueueUpdate 方法，这个方法能获取到每一个 DOM 方法最终修改的节点路径和值，如：{root.cn.[0].cn.[4].value: "1"}，并通过 setData 方法更新到视图层。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/297368/1658458460741-8cb7c7e9-ab7d-4a35-910b-9a1700c76c5d.png#clientId=u96ba7115-3cc6-4&from=paste&height=265&id=uf0ed8f51&name=image.png&originHeight=364&originWidth=729&originalType=binary&ratio=1&rotation=0&showTitle=false&size=149898&status=done&style=none&taskId=u53490226-1b24-4e6e-81da-89f2b7f420d&title=&width=531.5)

### Taro NPM
![image.png](https://cdn.nlark.com/yuque/0/2022/png/297368/1658458624594-3d7e4506-ec75-42d5-ad13-790cceb6eeea.png#clientId=u96ba7115-3cc6-4&from=paste&height=929&id=uabf7fb73&name=image.png&originHeight=929&originWidth=637&originalType=binary&ratio=1&rotation=0&showTitle=false&size=165954&status=done&style=none&taskId=u2ede6cc5-e263-48ac-bb1d-620a507cee5&title=&width=637)
