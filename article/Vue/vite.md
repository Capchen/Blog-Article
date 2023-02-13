## bundless
依据浏览器支持的ESM模块，去分别拉取对应的文件

- 冷启动快（使用esbuild-golang语言）
- 热更新快

## 创建构建流程

1. 创建koa服务
2. 使用chokidar监听文件
3. 模块解析
4. 增加各种中间件
5. 启动服务

## 静态文件托管

1. Vite会利用serverStaticPlugin将src和public目录设置为静态目录
2. serverStaticPlugin利用koa的koa-etag中间件打tag
3. devServer具备静态文件服务功能

## 模块路径重写

1. 处理裸导入： `import vue from vue` ，浏览器识别不了(只能识别相对路径或者绝对路径)
2. 对于非JS的请求路径，加上import参数 

## 静态资源打包策略

1. 获取vue内容，分别识别是否有template/style部分，有的话就发起请求
### CSS打包策略

1. 判断是否是.css文件的请求
2. 判断是否为js中的import css
3. 进行预编译处理
4. 基于css的源码包装成js模块
5. 创建style标签将css代码插入dom
6. 判断是否开启css module，是则导出对象否则导出css代码 

### 模板打包策略

1. 使用compiler-dom编译template ，然后返回浏览器
