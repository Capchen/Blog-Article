### commonJS规范
#### 模块
一个文件为一个模块，对模块的定义分为： 模块引用，模块定义，模块标识三个部分
##### 模块引用
`const chenjian = require(./chenjian.js)` 
##### 模块定义
上下文提供了export对象来导出模块，在模块中还存在一个module对象，它代表模块自身，所以export是
module的属性。
` export.say = function (){alert('hellow, i am good man')}`
##### 模块标识
模块标识其实就是传递给require的参数

![image.png](https://cdn.nlark.com/yuque/0/2019/png/297368/1558345821818-aa74a9b4-861b-4087-a477-f4571de85cbd.png#align=left&display=inline&height=242&name=image.png&originHeight=242&originWidth=556&size=24200&status=done&width=556)


### node 对模块的实现
 node中模块的实现还是和commonsJS有些不同的
引入一个文件，node会进行
1.文件路径分析
2.文件定位
3.文件编译
node中的那些内置模块在node进程运行的时候就已经编译到内存中了，所以内置模块的获取和访问是最快的

node对模块的访问会和浏览器一样对对象进行缓存，但是浏览器缓存的对象是文件，而node则是编译和执行之后的
对象


1.**node对CommonJs的实现**：事实上，在编译的过程中，Node对获取的JavaScript文件内容进行了头尾包装。在头部添加了(function (exports, require, module, __filename, __dirname) {\n，在尾部添加了\n});。
一个正常的JavaScript文件会被包装成如下的样子

```
(function (exports, require, module, __filename, __dirname) {
  var math = require('math');
  exports.area = function (radius) {
    return Math.PI * radius * radius;
  };
});
```

这就是这些变量并没有定义在每个模块文件中却存在的原因。在执行之后，模块的exports属性被返回给了调用方。

2.Node调用process.dlopen()方法进行加载和执行。在Node的架构下，dlopen()方法在Windows和*nix平台下分别有不同的实现，通过libuv兼容层进行了封装。

3.**模块类型的层级关系：**

内建模块（纯c/c++编写的模块） ===> 核心模块（部分c/c++编写，由javascript封装的模块，Node的buffer、crypto、evals、fs、os等模块都是部分通过C/C++编写的。） ===> 文件模块（指开发者自己创建的文件）

### node


