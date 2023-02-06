### 什么是微前端
引用[https://hackernoon.com/understanding-micro-frontends-b1c11585a297](https://hackernoon.com/understanding-micro-frontends-b1c11585a297)这篇文章的定义
> Micro Frontends背后的想法是将网站或Web应用视为独立团队拥有的功能组合。每个团队都有一个独特的业务或任务领域，做他们关注和专注的事情。团队是跨职能的，从数据库到用户界面开发端到端的功能。（micro-frontends.org）

也可以简单的理解为：微前端是一种利用工具拆分来达到工程拆分治理的方案。可以解决工程膨胀/开发维护困难等问题。

映射到我们的BF：产品模块现在已经有9，10个，产品模块里面有可能还包含更复杂的业务，所以需要拆分，现在我们也是这么做的。

### 微前端需要解决的问题
#### 多个独立的子业务如何统一UI体验（CSS和公共组件）
#### 微应用之间如何共享全局信息（登陆信息，证书过期信息）
#### 微应用中的路由如何管理（解析加载对应微应用资源）

### BF是如何处理这些问题的
BF中产品和业务的拆分，肯定绕不过上面三个问题。我们可以看BF是如何处理，并思考哪些可以改进。
先来一张前端工程架构概述图：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1594179896964-fd4bdb2d-5eeb-4b2e-aa96-6cfa7885b90d.png#align=left&display=inline&height=414&name=image.png&originHeight=413&originWidth=658&size=39803&status=done&style=none&width=659)

#### 第一个问题：
统一UI，BF使用 `elementUI` 来作为一些基本组件的开发。但是重新设计了自己的UI风格。并且封装了一些公共组件，包括：素材库，panel等。这些公共组件和css都是在common-module里面。common-module作为一个应用独立部署。可以看上图。
我们在微应用中的使用的方式就是这样
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1594180804660-892e4b4a-667a-4ca7-bc5d-8118b50ae021.png#align=left&display=inline&height=133&name=image.png&originHeight=266&originWidth=1312&size=63916&status=done&style=none&width=656)
和
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1594180872247-37cca75f-9f2b-4027-9d5e-d9364fdb651e.png#align=left&display=inline&height=478&name=image.png&originHeight=956&originWidth=1042&size=170326&status=done&style=none&width=521)

#### 第二个问题：
微应用之间没有通讯机制，一些用户信息，授权信息，都是通过一进来就调用接口获取。存储方面就是在localstorage中进行数据的交换同步

#### 第三个问题：
产品模块的入口，都是在bf页面的左边。通过在表中插入信息，在点击事件的时候跳转到不同的应用。跳转的代理，就轮到上图的load-balance上场了。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1594204409981-f4e979fa-8005-4d16-b9d4-4429ce13d9a9.png#align=left&display=inline&height=243&name=image.png&originHeight=486&originWidth=1070&size=71500&status=done&style=none&width=535)
load-balance里面就是一个nginx，专门用来做各种代理转发。可以看到上图就是对ccbot.html这个路径转发到ccbot/#/，而/ccbot又转发到了定义好的ip+端口的服务上，也就是启动的docker上（docker集群中每个docker容器都会暴露出ip+端口）
进入到微应用之后，就是通过docker容器内的nginx去处理接口的转发和代理了。
