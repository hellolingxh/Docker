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
- Dock Daemon: API and Features
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

