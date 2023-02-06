### 背景：
H5聊天机器人配置功能，希望在demo-test环境中生效，但是暂时还不希望跟着BF2.1.4版本。demo-test环境又是和2.1.4版本同步的，所以如果要在两个环境中差别维护H5配置这个功能,在升级或更新时需要额外的精力对这个模块做处理。所以希望通过能否在部署工程中配置个开关，对这个功能部署时控制是否展示。

### 技术实现原理：
原理和设置nginx配置文件一样，前端先定好一个模版，通过变量注入的方式来获取对应变量值。理解前端工程从构建到启动容器这个阶段，就很好理解这种实现方式。

### 文件说明：
#### common_variable.js.template
获取配置变量的模版文件，和nginx.conf.template在同一目录下，这个文件的作用就是作为一个js文件在项目加载的时候一起加载，定义为一个自执行函数，如下图：把获取到ADMIN_IS_OPEN_H5_CHATBOT变量放入到deployVariable对象中，并且将对象挂载到window下。这样在项目中就可以通过访问window.deployerVariable访问到想要的变量。且deployerVariable为只读。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1600227029044-3abfa65c-aa67-407a-a267-9cad05a59a01.png#align=left&display=inline&height=202&name=image.png&originHeight=404&originWidth=1000&size=57857&status=done&style=none&width=500)

#### run_nginx.sh
这个文件是启动容器时执行的一个脚本。通过执行这个文件，将配置变量传给nginx文件。如下图：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1600227373479-edf149a8-1ce4-454a-b595-edfa191fd60c.png#align=left&display=inline&height=497&name=image.png&originHeight=994&originWidth=864&size=139034&status=done&style=none&width=432)
意思是以nginx.conf.template为模版文件写入nginx.conf文件，并且将前缀为$$ 的变量注入，这样在nginx.conf.template文件中定义的同名变量就能被在部署工程中定义的具体的值取代。


#### Dockerfile
打包docker镜像的配置文件，里面定义基础镜像，切换工作目录，拷贝文件等操作步骤。

#### index.html
前端单页面应用的入口文件，一切的起源。包括我们的公共组件，element，vue都是通过这个文件加载。如下图：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1600227832447-5aa44d39-53c9-46d2-80b0-09991d96cc6b.png#align=left&display=inline&height=457&name=image.png&originHeight=914&originWidth=1578&size=252472&status=done&style=none&width=789)
### 操作步骤：
我们涉及到修改的就是以上四个文件
#### 第一步：
在docker/data/conf文件夹下增加common_variable.js.template文件，这个模板文件代码如上。需要添加变量的话只要在
```javascript
(function(window){
    const deployVariable = {
        ADMIN_IS_OPEN_H5_CHATBOT: "${ADMIN_IS_OPEN_H5_CHATBOT}"
        //在这里添加新的变量，如:需要添加is_open_image
      	ADMIN_IS_OPEN_IMAGE: "..."
    }
    Object.freeze(deployVariable);
    window.deployVariable = deployVariable
    Object.defineProperty(window, 'deployVariable', {
        writable: false
    })
})(window)
```

#### 第二步：
在Dockerfile文件中加入
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1600238533099-95fdfc2c-c34c-4471-bec2-0842dc58ecea.png#align=left&display=inline&height=287&name=image.png&originHeight=574&originWidth=1562&size=117100&status=done&style=none&width=781)
Dockerfile的执行是在run_nginx.sh脚本之前，加入这一行的目的为了将我们的模板文件，拷贝到docker容器的相关目录下，为下一步做准备。

#### 第三步：
在run_nginx.sh脚本中加入如下代码
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1600238748977-04c5c64f-6f8f-4900-abfe-74ea826e6390.png#align=left&display=inline&height=378&name=image.png&originHeight=756&originWidth=1512&size=113623&status=done&style=none&width=756)
如上，在运行run_nginx.sh脚本的时候，会先把工作目录切换到前端静态资源的目录  `dist/static/js`
如果已经存在common_variable.js文件会先删除，然后使用 `envsubst` 注入部署工程的变量，以template模板文件在目标目录生成common_variable.js文件。
这时在common_variable.js文件中，你在template文件中设置的变量占位符就会替换成部署工程中的具体的值。

#### 第四步：
index.html文件加入如下代码
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1600239187818-20cfd5a8-d841-4057-a3f1-8aaaadb13277.png#align=left&display=inline&height=266&name=image.png&originHeight=532&originWidth=1548&size=178712&status=done&style=none&width=774)
上面三部我们做的操作是：在前端静态资源构建完成之后，利用模板文件在 `static/js` 目录下生成了真正的common_variabel.js文件，而部署工程的变量就是在生成真正的js文件的时候注入的（和生成真正的nginx配置文件一样的原理）。
这一步我们就是使用这个文件，作为资源在html文件中加载。加载完之后，就可以在window上看到deployVariable这个属性了。然后在项目中需要的地方拿来用就好了


### 总结：
一句话概括：我们就是先把部署工程的变量拿到放在一个js文件中，然后再通过项目加载这个js文件来将变量挂载到window上。这样我们就可以在项目中使用这些变量。
目前在general-ui的patch/2.1.4上已经更新。

### 问题：
目前暴露出的问题且可优化的点：
1.这个js文件不经过webpack打包，是个没有经过混淆和压缩的'明文'代码
2.在更新变量状态部署之后，需要重新刷新浏览器生效，因为依赖于文件是否执行
...

