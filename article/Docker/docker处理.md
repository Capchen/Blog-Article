### 前端docker容器
前端的镜像里面包含nginx服务，外部请求过来后，会先经过load-balance（以下简称LB）来进行一个转发，比如技能开发的URL为/skill/，LB会将此路径的请求转发到skill-ui容器，skill-ui容器里面nginx会依据配置，将/skillAPI/路径下的请求转发到配置好的skill_backend_url容器。
下图是网页的一个请求：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1590117726141-0eb1c90c-c1c7-4b5f-8c8e-0e8923653de6.png#align=left&display=inline&height=144&name=image.png&originHeight=144&originWidth=635&size=17876&status=done&style=none&width=635)
下图是skill-ui中nginx的配置：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1590117547511-c3979fc2-2ad1-4ff6-9fca-a6acecfa45a0.png#align=left&display=inline&height=674&name=image.png&originHeight=674&originWidth=1446&size=90942&status=done&style=none&width=1446)

所以可以大概得出：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/297368/1590119860165-04b287ed-c585-44a7-a416-32a3f59940c5.png#align=left&display=inline&height=317&name=image.png&originHeight=462&originWidth=811&size=25120&status=done&style=none&width=556)

### docker 容器集群
每个容器都有自己的虚拟ip，docker会有类似网关的管理者，比如skill-backend容器重启之后，它的虚拟ip会变化，这时路由管理者就会重新去注册新的ip路由表，保证与其他容器的正常连通。
通过设置netword来将多个容器组成一个局域网
#### 健康检查
每个容器都可以设置一个健康检查，每隔一段时间去curl当前容器内的nginx端口服务。

#### swarm
比如现在有1，2，3个服务器。a服务是部署在1上的。如果a挂了，那么就会在2上重新部署a

