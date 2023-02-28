# Tmagic-Editor工具调研

- [文档地址](https://tencent.github.io/tmagic-editor/docs/guide/)
- [Github源码地址](https://github.com/Tencent/tmagic-editor)
- [线上体验地址](https://tencent.github.io/tmagic-editor/playground/index.html#/)

## 背景

随着公司业务矩阵的扩展，加上内容团队的宣传活动需求等，未来可预见会有大量的重复工作需求。如何降低前端开发人力投入，如何快速响应内容团队的需求，就需要考虑是否有工具能满足我们的诉求。

## Tmagic-Editor

腾讯开源的一款可视化搭建平台，定位是生成移动端H5页面。支持本地部署，自定义构建

### 功能特性

<!-- ![能力项](https://gyenno-test.oss-cn-beijing.aliyuncs.com/pub/miniprogram/demo-test/tmagic-editor-power.png) -->


![概览](https://img-blog.csdnimg.cn/img_convert/ec7d52bbb2b07a0b79e3f6d1de7effeb.png)

#### 一、所见即所得

拖拽编辑方式，包含了【左面板，工具栏，工作区，右面板】这四个操作空间。

![示例图片-1](https://vfiles.gtimg.cn/vupload/20211009/083dfa1633771059332.png)

#### 二、二次开发的拓展能力

支持自定义组件、插件，自定义部署流程。  

- 编辑器使用vue3开发，组件支持以（vue3/vue2/react）的方式开发，接入成本相对较小。
- 可以通过插件实现类似登录，页面环境判断，请求拦截器等等功能。
- 可以接入公司的jenkins构建平台

#### 三、支持组件联动的能力

支持组件通信、组件联动，允许页面内各组件提供丰富配置能力。

#### 四、支持多种布局

他有容器的概念，容器可以无限嵌套，支持流式布局和定位布局，丰富了布局方式。

#### 五、DSL源码

配置保存后生成的产物是DSL源码，可以直接编辑DSL源码去修改页面样式。

#### 六、提供tmagic-admin的管理后台

包含的功能有新建活动，版本管理，版本回退，一键发布等等，还可以自己扩充。

### 技术实现

#### 名词解释

- `runtime`: 承载项目页面的运行环境。（编辑器工作区是一个runtime，发布上线后用户访问的页面也是一个runtime）

- `packages`: 一些依赖包，包括了ui/cli/editor等

- `playground`: 是一个编辑器项目示例，使用了`package`和`runtime`内容的集成项目

#### 介绍

项目的核心是`packages/editor`中的编辑器，编辑器包括了：  

1. 模拟器：居中位置的内容，模拟真实页面的展示内容

2. 组件库：左侧的组件列表

3. 组件树：树状结构展示组件层级

4. 表单配置：右侧的配置项

5. DSL源码：通过工具栏的源码按钮生成  

**布局原理**  
上文说到有容器的概念，布局的关键就是将布局配置指定在容器上，对容器内的所有子组件生效。

**页面渲染原理**  
上文说到配置保存的产物是DSL源码，由这份源码通过UI渲染器去生成组件。DSL又是根据布局原理的方式去生成的，所以DSL也是树状结构，通过递归的方式去渲染。参考下面官方提供的图
![示例图片-2](https://vfiles.gtimg.cn/vupload/20211009/f4d3031633778551251.png)

**UI渲染器原理**  
上面说到页面渲染是通过DSL，经由UI渲染器，渲染成页面。UI渲染器是怎么知道他该渲染哪个组件？因为DSL中的type是唯一的，与组件type是映射关系。所以通过type匹配到对应组件，由vue的`component`组件去动态匹配。基于这个原理，就需要我们的type命名唯一。

**UI渲染器和runtime**  
因为工具想要做到渲染器和editor的解藕，所以渲染的工作不在编辑器，那么编辑器中的模拟器部分是如何模拟真实页面的展示内容的？先说上文对`runtime`的解释是：承载项目页面的运行环境。所以`runtime`和`UI渲染器`的关系就是：`runtime` 需要一个对应的 UI 来作为渲染器，这样`runtime`才能解析DSL。而编辑器中的模拟器是通过 `iframe` 渲染的,这个`iframe`就是runtime提供出来的
> @tmagic/ui 和 runtime 是配套出现的，runtime 必须基于 @tmagic/ui 才可以实现渲染。因为 @tmagic/ui 需要提供 runtime 所需要的渲染器,现在有vue/vue3/react。也可以自己实现其他前端框架的实现。

**runtime与editor通信**  
既然是`ifrmae`的方式，那么runtime和editor就需要通信。主要是通过editor注入到window的magic api来进行交互。这里需要注意的UI渲染器也会将页面dom同步给编辑器。
详细参考官方文档[runtime与editor通信](https://tencent.github.io/tmagic-editor/docs/guide/tutorial/runtime.html#runtime%E4%B8%8Eeditor%E9%80%9A%E4%BF%A1)

**组件联动原理**  
以vue3组件为例，组件的事件绑定/分发，是在`packages/core`中实现。组件渲染时，会把组件声明的事件（注册组件的时候根据规则，配套会有一个`event.ts`文件，在这个文件中声明当前组件的事件相关内容。）提供给`core`，`useApp`这个方法就是干这个事的。在vue的实现中useApp是将整个vue实例都提供给了app，所以需要defineExport来定义vue instance上的方法。

简单可以理解为:把所有组件的事件都暴露给vue实例，所以事件名必须唯一（用命名空间区分），然后在需要联动的组件里面匹配触发

### 生产构建

#### 第一步： 构建生产环境使用的runtime  

``` 

# 输出的dist是playground示例web，输出的publist是对应的runtime
npm run build:playground

```

1. page.html的使用：可以拷贝很多份，然后将dsl.js插入进去来使用(window.magicDSL=[])  
2. playground.html的使用： 示例项目的runtime

#### 第二步： 将第一步的构建产物进行归档编号，存为版本

#### 第三步： 将page.html发布到指定位置，提供访问

### 适用场景

#### 静态H5页面

- 官网demo
- 宣传页面

#### 能抽象出来的固定业务流程

场景一： 用户点击领取礼包->弹出手机号，验证码->确认  
场景二： 匿名问卷页面，匿名投票页面

### 不适用场景

#### 混合开发场景

场景一： app内嵌的购药页面，包含大量逻辑判断，方法调用  
场景二： 内嵌微信。需要调用微信能力的页面

#### 做一个完善的h5应用

完善的h5应用包括了用户体系，权限校验，复杂的页面跳转，全局状态管理等等。

### 如果我们自己使用，还需要待完善的地方

1. 需要生产可用，需要花时间整合进入CI，并对管理后台做修改
2. 自定义组件要额外开发
3. 业务插件要额外开发
4. 探究UI库的接入使用

### 历史版本

[Github上的更新记录](https://github.com/Tencent/tmagic-editor/releases)

<!-- ![历史版本](https://gyenno-test.oss-cn-beijing.aliyuncs.com/pub/miniprogram/demo-test/tmagic-editor-history-release.png) -->

### 未来特性

从[Issues](https://github.com/Tencent/tmagic-editor/issues/481)和QQ群中了解到，动态数据源（api请求等）这个feature官方已经在讨论中，还处于设计阶段，估计会被官方支持。

但是就算官方不支持，其实从上面的issues中也可以看出，自己可以编码实现这个功能。

### 社区支持

- 文档方面：更新较慢，有些模块的介绍文档还不全
- PR相对较少，但合入速度很快
- 有官方qq群，每天都有人提问题，官方小助手回复消息也很快。
