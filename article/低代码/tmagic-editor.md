# tmagic-editor

# 可视化编辑器的实现思路

## 编辑器

- 编辑器可以理解为就是操作页面，分为左面板、工作区、头部工具栏和右面板。
    
    一般左面板是等待拖拽的组件，工作区是接收组件的地方，右面板是调整组件
    
    细节的地方，例如样式/事件等。
    

工作区的画布如果需要理解左面板的组件，就需要拿到具体的函数去render，这里用到了

动态编译vue模版

## Runtime

- Runtime就是指能实时显示你在工作区操作的结果，tmagic-editor为了实现render函数和编辑器解藕，所以不使用render的方式

Runtime的实现是新的一个工程，采用runtimeUrl的iframe嵌入方式去和编辑器互动。Runtime要渲染编辑器的组件，就需要拿到编辑器的dsl，那么就是通过监听message的方式去进行通信。

而且runtime也需要将dom同步给编辑器，不然编辑器不知道此刻你选择的是哪个组件

## DSL

- tmagic-editor提供了三个版本的解析渲染组件（vue3/vue2/react），渲染组件**ui-xx**
    
    是和Runtime对应的，每个DSL都需要对应的Runtime去解析。
    

DSL解析的实现：通过component组件的 **is** 属性去动态渲染。例如。左面板有个type为my-button的组件，那么DSL里面就一定有一个组件名为my-button的组件。这是映射关系。