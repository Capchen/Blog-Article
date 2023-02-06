### 背景

前端已经在新的PC端项目上使用`vite+vue3+ts+pinia+vue-router`技术栈进行开发(UI库不一样)，但是并没有形成一个统一配置模板。未来开发人员切换项目的时候可能会有一定的熟悉成本，而且在在新的PC端项目开始的时候，还需要再来一次项目初始化。尽早对PC端模板达成一致，可以尽早在项目中实践。

### 期望

- PC端能统一模板
- 整理出一套模板后，再在此基础上开发gyenno-cli，达到脚本自动化初始化项目

### 如何形成较稳定的统一模板

PS.形成统一模版是默认基于当前`Vite, Vue3, TypeScript, Pinia`技术栈进行的，也就是默认技术栈已经统一。

#### 方案1.根据公司现有的PC端项目去做整理归纳

因为是针对新技术栈的模板，所以无法根据历史项目（vue2）去抽取模板，且未来都会基于vue3+ts生态开发新项目。

如果是根据现在进行的基于新技术栈开发的PC端项目（`基础数据服务` 和 `睿云管理后台`）去抽取的话：

---

优:

1. 更贴合业务的开发模版
2. 开发人员更熟悉

---

劣:

1. 有试错成本（新技术栈的试错）
2. 随着项目去走，抽取的周期会长

#### 方案2.找到成熟稳定的开源项目来抽取模板，

有一个稳定的项目且基于新技术栈开发，对于新技术不熟悉的开发人员都是不错的参考。

1. 可以参考更好的方法函数封装（例如基于ts的axios封装请求库）
2. 可以参考更全面的TS封装和写法指导
3. 可以参考一些业务场景的解决方案

但是如何评价这个项目的代码质量或者封装抽取组件思维是否达标，也是仁者见仁，更多的还需要不断的深入才能给出答案。

个人认为：有这么一个项目存在，现阶段来说是利大于弊的。可以在过渡阶段起到较大的作用。也能帮助提升形成模板的效率。且随着新技术栈的开发经验提升，也可以不断优化模板。

### 基于第2种方案

基于第二种方案的尝试，有以下这些模板，技术栈和当前的`基础数据服务` 和 `睿云管理后台`两个项目匹配，UI库的不同不影响基础模版。

#### vue-vben-admin开源模板（Vite, Vue3, Ant-Design-Vue, TypeScript, Pinia）

- [完整版Github](https://github.com/vbenjs/vue-vben-admin.git)
- [精简版Github](https://github.com/vbenjs/vben-admin-thin-next)
- [完整版演示地址](https://vvbin.cn/next/#/login?redirect=/dashboard)
- [中文文档地址](https://vvbin.cn/doc-next/)

> 区分完整版和精简版，完整版功能更多，插件更多；精简版可根据自己的需求添加插件


##### 文档层面

有介绍较全的中文文档，覆盖了从浅到深的介绍。

有演示站点，方便调试

##### 源码层面

项目在持续的更新，文件层级清晰，源码部分有注释

##### 其他点

- Issues较多（主要集中在功能问题）
- UI库是基于Ant Design

#### vue-pure-admin开源模板（Vite, Vue3, TypeScript, Tailwind CSS, element-plus）

- [完整版Github](https://github.com/xiaoxian521/vue-pure-admin)
- [精简版Github](https://github.com/xiaoxian521/pure-admin-thin)
- [完整版演示地址](https://vue-pure-admin.vercel.app/#/login)
- [中文文档地址](https://pure-admin-doc.vercel.app/)

在文档/源码层面大同小异

##### 亮点

1. 使用了`Tailwind CSS`,对css这块有一定的参考价值

#### vue-naive-admin开源模板（Vite, Vue 3, Pinia, Naive UI）

- [Github](https://github.com/zclzone/vue-naive-admin)
- [中文文档地址](https://zclzone.github.io/vue-naive-admin-docs/)

##### 亮点

1. 相对精简，适用于中小型项目
2. 原子化 css 解决方案

### 结论

1. 我们现阶段目标，是生成一个PC端的基础模版，这样在未来的新项目就可以应用起来且可以不断丰富。
2. `基础数据服务` 和 `睿云管理后台` 已经在进行开发中，结合方案1和方案2，基于这两个项目再配合参考 上述`vue-vben-admin`等模板 ，可以较全面且快速的生成一个基础试错模板。
3. 对`vue-vben-admin`等模板保持借鉴参考的态度，取其精华去其糟粕。
