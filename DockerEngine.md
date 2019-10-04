# Docker Engine 简介
- Docker Engine是用来运行和管理容器的核心软件
- 采用模块化的设计原则，其组件都是可替换的
- 这些模块共同负责容器的创建和运行，它们包括：
  - Docker Client
  - Docker Daemon
  - containerd
  - runc
  
# Docker Engine 变迁
- Docker 首次发布时，Docker Engine由两个核心组件构成
  - LXC: provides communicate capacity of Namespace and CGroup both base on Linux kernel LXC 
  - Docker daemon: monolithical binary module that includes Docker client, Docker API, containerd, 镜像构建等　
  - LXC only runs on Linux that cannot across multiple platforms, it was insteaded of Libcontainer as default
  - In the future, Docker may support other container formats by integrating with technologies such as BSD Jails or Solaris Zones
  - Decoupling monolithical Docker Daemon into multiple modules
  
# Docker Engine 当前架构
- Docker Clinet: Docker Commands (CLI)
- Docker Daemon: API and Features
- containerd: container supervisor (start|stop|pause...)
- shim: 启动无守护进程容器
- runc: 容器运行时（内核原语接口）

# OCI
- image standard
- runc standard

# runc
- Docker Engine 实现了OCI规范，包括：
  - Docker daemon不再包含任何容器运行时的代码
  - Docker使用runc来实现容器运行时，它只有一个作用就是用来创建容器
  - runc实质上是一个CLI包装器，一个独立的容器运行时工具，并不提供类似Docker Engine所拥有的丰富功能
  - runc实质上是一个轻量级的，针对Libcontainer进行了包装的命令行交互工具
  - runc有时也被称作OCI层
  - Libcontainer用来取代早期Docker架构中的LXC
  - containerd组件确保了Docker image能够以正确的OCI Bundle的格式传递给runc

# containerd
- 所有的容器执行逻辑被放在该工具中
- 主要任务是容器的生命周期管理
  - start
  - stop
  - pause
  - rm
  - ...
- 可以跨平台并以daemon方式运行
- 位于Docker daemon和runc所在的OCI层之间
- 随着时间推移，它不仅仅只是容器的生命周期管理，而是赋予以更多功能，比如镜像管理

# 如何启动一个新容器
- 常用的方式即使用Docker命令行工具
```
$ docker container run --name ctr1 -it alpine:latest sh
```
- 当使用Docker命令行工具执行该命令时，Docker client会将其转换为合适的API调用
- API是在Docker daemon中实现的，基于REST API的容器API
- 一旦Docker daemon接收到创建新容器的命令时，就会向containerd发出调用。
- containerd并不负责具体的创建容器的工作，而是命令runc去做
- containerd将Docker 镜像转换为OCI bundle，并让runc基于此创建一个新的容器
- runc与操作系统内核接口进行通信，基于必要的工具(Namespace,CGroup等)来创建容器
- 创建的容器作为runc的子进程启动，启动完毕后，runc将会退出
- 该命令执行流程如下：
  - Docker Client: 向Docker API发出docker container run命令
  - Docker daemon: 在API endpoint接收指令，指示containerd启动新容器
  - containerd: 给runc传递OCI Bundle(镜像), 指示func创建容器
  - shim & runc: 创建容器
  
# Docker Engine解耦的优势
- 将所有的用于启动，管理容器的逻辑和代码从Docker daemon中移出,容器运行时称为“无守护进程的容器”，对Docker daemon的维护和升级工作不会影响到运行中的容器
- 而在旧模型中，所有容器运行时的逻辑都在Docker daemon中实现，启动和停止daemon会导致宿主机上所有运行中的容器被杀掉。这在生产环境中是一个大问题。

# shim
- shim 是实现无daemon的容器，用于将运行中的容器与daemon解耦
- 之前提到,containerd指示runc来创建容器，事实上每次创建容器时它都会fork一个新的runc实例。一旦容器创建完毕，对应的runc进程就会退出。
- 一旦容器进程的父进程runc退出，相关联的containerd-shim进程就会成为容器的父进程
- 作为容器的父进程, shim部分职责如下:
  - 保持所有STDIN和STDOUT流是开启状态，从而当daemon重启的时候，容器不会因为管道pipe的关闭而终止
  - 将容器的退出状态反馈给Docker daemon
  
 # Docker Engine在Linux中的实现
 - Linux系统中Docker Engine组件名
    - dockerd
    - docker-containerd
    - docker-containerd-shim
    - docker-runc
 - Linux系统中可执行ps命令查看以上进程，有些进程只有在运行容器的时候才可见
 
 # Docker daemon的作用
 - 镜像管理
 - 镜像构建
 - REST API
 - 身份验证
 - 安全
 - 核心网络
 - 编排
 - Docker Engine的模块化工作仍在进行中

  

