# Docker Container
- 容器是镜像的运行时实例
- 虚拟机和容器最大区别是容器更快更轻量级
- 虚拟机运行在完整的操作系统之上，而容器会共享其所在主机的操作系统内核
- 运行容器命令
  - docker container run <image> <app>
    - $ docker container run -it ubuntu /bin/bash
    - 该命令会启动Ubuntu Linux容器，并运行Bash Shell作为该容器内运行的应用
    - 参数 *it*　可以将当前终端连接到容器的Shell终端之上
    - 容器随着其中运行应用的退出而终止
- 停止容器运行的命令
  - $ docker container stop
- 启动容器的命令
  - $ docker container start
- 删除容器的命令
  - $ docker container rm
  
## 容器 vs 虚拟机
