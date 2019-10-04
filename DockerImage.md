# Docker 镜像
- Docker Image可以理解为Class,而容器可以理解为Instance
- 镜像可以理解为构建时结构,容器可以理解为运行时结构
- 可以从镜像仓库拉取镜像,常见的镜像仓库服务是Docker Hub,不过也可以使用其他的镜像库
- 拉取操作会将镜像下载到本地Docker主机,可以使用该镜像启动一个或者多个容器
- 镜像由多个层组成,每层叠加后,从外部看来就如一个独立的对象
- 镜像内部是一个精简的操作系统(OS),同时还包含应用运行的文件和依赖包.
- 使用docker container run 和 docker service create命令从某个镜像启动一个或多个容器.
- 一旦容器从镜像启动后,二者就互相依赖,在镜像上启动的容器停止之前,镜像是无法被删除的,否则将出现以下错误:
  - Error response from daemon: confilict:unable to remove repository reference (must force) - container is using this referenced image
- 镜像通常很小,不包含内核,容器都是共享所在的Dock Host的内核
- 容器仅包含必要的操作系统(通常只有操作系统文件和文件系统对象)

# 拉取镜像
- Linux Docker Host本地镜像仓库通常位于/var/lib/docker/<storage-driver>
- 查看Docker Host是否存在镜像
  - $ docker image ls
- 拉取Docker 镜像到本地
  - $ docker image pull ubuntu:latest
 
# 镜像仓库服务
- Docker镜像存储在镜像仓库服务(Image Registry)当中
- Docker客户端的镜像仓库服务是可配置的,默认使用Docker Hub
- 镜像仓库服务包含多个镜像仓库(Image Repository),同样一个镜像仓库可以有多个镜像
- Docker Hub也分为官方仓库(Official Repository)和非官方仓库(Unofficial Repository)
- 官方仓库中的镜像由Docker公司审查的,这意味着其中的镜像会及时更新并由高质量,安全,完善的文档,最佳实践
- 镜像仓库服务,仓库以及镜像关系
  - 镜像仓库服务 1 -----> * 仓库
  - 仓库 1 ------> * 镜像

# 镜像命名和标签
- 官方仓库镜像定位 <repository>:<tag>. 如果未指定<tag>, 则Docker会假设用户希望拉取标签为latest的镜像
  - e.g. $ docker image pull alpine:latest
  - e.g. $ docker image pull ubuntu:latest
  - e.g. $ docker image pull mongo:3.3.11
  - e.g. $ docker image pull redis:latest
  - e.g. $ docker image pull alpine
- 标签为latest的镜像并没有什么特殊, 标有latest标签的镜像不保证就是仓库最新的镜像
  - e.g. Alpine 仓库中最新的镜像通常标剑是edge
- 使用非官方仓库拉取镜像也是类似,只需在仓库名称前面加上Docker Hub的用户名或组织名称,例如:
  - e.g. $ docker image pull nigelpoulton/tu-demo:v2
    - 该命令会从以我自已的Docker Hub帐号为命名空间的tu-demo库中下载标签下载v2的镜像
- 如果希望从第三方镜像仓库服务获取镜像(非Docker Hub),则需在镜像仓库名称前加上第三方镜像仓库服务的DNS名称
  - e.g. docker pull gcr.io/nigelpoulton/tu-demo:v2
  
# 为镜像打多个标签
- 一个镜像可根据用户需要设置多个标签.这是因为标签存放在镜像元数据中的任意数字或字符串
- docker image pull -a 用来拉取仓库中的全部镜像.
  - 如果拉取的镜像仓库中包含用于多个平台或者架构的镜像,比如同时包含linux和windows的镜像,那么命令可能会失败
  - e.g. $ docker image pull -a nigelpoulton/tu-demo
- 可以存在多个标签指向同一个镜像
- 那些没有标签的镜像被称为悬虚镜像,在列表中展示为<none>:<none>
  - 通常出现这种情况,是因为构建了一个新镜像,并为该镜像设置了一个已存在的标签

# 过滤docker image ls的输出内容
- Docker 提供 --filter参数为过滤docker image ls 命令返回的镜像列表内容
  - docker image ls --filter dangling=true
