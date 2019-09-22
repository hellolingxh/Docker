# Docker 简介
- 1 是一种运行于Linux和Ｗindows上的软件，用于创建，管理和编排容器。
- 2 开源项目叫做Moby on Github
- 3 主要有两个版本：EE & CE
- 4 Features:Batteries included but removable
- 5 两个主要组件：Docker Client & Docker Daemon. 它们之间的通信是通过本地IPC/UNIX Socket完成的(/var/run/docker.sock)
- 6 查看详细版本信息执行: docker verion
- 7 可以使用 ``` docker version ``` 来检测客户端和服务端是否都已经成功运行, 
- 7 主要使用Go语言实现

# Docker Architecture
![](https://docs.docker.com/engine/images/architecture.svg)

# 开放容器计划 OCI
- 1 旨在对容器基础架构中的基础组件(如镜像格式与容器运行时)进行标准化的管理委员会
- 2 在Linux基金会的支持下运作，Docker公司与CoreOS公司都是主要贡献者

# Docker 安装 on Ubuntu
## Set up Docker's repositories
- 1 Update the **apt** package index:

  ``` $ sudo apt-get update ```
  
- 2 Install packages to allow **apt** to use a repository over HTTPS:

  ``` 
  $ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common 
  ```
  
- 3 Add Docker's official GPG key:

  ``` 
  $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - 
  ```
  
- 4 Verify that you now have the key with the fingerprint using 

  ```
  $ sudo apt-key list
  
  pub   4096R/0EBFCD88 2017-02-22
  uid                  Docker Release (CE deb) <docker@docker.com>
  sub   4096R/F273FCD8 2017-02-22
  ```
  
- 4　By searching for the last 8 characters of the fingerprint

  ``` 
  $ SUDO apt-key fingerprint OEBFCD88 
  pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
  uid                  Docker Release (CE deb) <docker@docker.com>
  sub   4096R/F273FCD8 2017-02-22 
  ```
  
- 5 Use the following command to set up the **stable** repository. To add the nightly or test repository, add the work ``` nightly ``` or ``` test ``` (or both) after the work ``` stable ``` in the commands below.

  ```
  $ sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"
  ```
- 6 Update the **apt** package index.

  ``` $ sudo apt-get update ```
 
- 7 Install the latest version of Docker Engine - Community and containerd
  
  ``` $ sudo apt-get install docker-ce docker-ce-cli containerd.io ```
  
- 8　Or go to the next step to install specific version
  - List the versions available in your repo:
  
    ```
    $ apt-cache madison docker-ce
     docker-ce | 5:19.03.2~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     docker-ce | 5:19.03.1~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     docker-ce | 5:19.03.0~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     docker-ce | 5:18.09.9~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     docker-ce | 5:18.09.8~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     docker-ce | 5:18.09.7~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     docker-ce | 5:18.09.6~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     docker-ce | 5:18.09.5~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     docker-ce | 5:18.09.4~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     docker-ce | 5:18.09.3~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     docker-ce | 5:18.09.2~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     docker-ce | 5:18.09.1~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
     docker-ce | 5:18.09.0~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
    ```
  - Install a specific version using the version string from the second column, for example, ``` 5:19.03.2~3-0~ubuntu-xenial ```
  
    ```
    $ sudo apt-get install \
      docker-ce=5:19.03.2~3-0~ubuntu-xenial \
      docker-ce-cli=5:19.03.2~3-0~ubuntu-xenial \
      containerd.io
    ```
- 9 Add the user into the group ``` docker ```

  ``` $ sudo usermod -aG docker gray ```
  
- 10 Verify that Docker Engine - Community is installed correctly by running the ``` hello-world ``` image.
  
  ``` $ sudo docker run hello-world ```


## Download the Deb package and install it manually

## Use automated convenience scripts to install Docker in testing and development environment

# Docker 升级 on Ubuntu

# Docker 存储驱动
- 1 每个Docker容器都有一个本地存储空间,用于保存层叠的镜像层(Image Layer)以及挂载的容器文件系统
- 2 默认情况下，容器的所有读写操作都发生在其镜像层上或挂载的文件系统上，所以存储是每个容器的性能和稳定性不可或缺的一个环节
- 3 Docker在Linux底层支持几种不同的存储驱动的具体实现，每一种实现方式都采用不同方法实现了镜像层和写时复制(Copy-on-Write)，对Docker性能和稳定性至关重要。这些存储驱动包括：
  - AUFS
  - Overlay2
  - Device Mapper
  - Btrfs
  - ZFS
- 4 Docker在Windows操作系统上只支持一种存储驱动，即 ``` Windows Filter ```
- 5 存储驱动的选择是节点级别的，每个Docker主机只能选择一种存储驱动，而不能为每个容器选择不同的存储区动
- 6 可以通过修改/etc/docker/daemon.json文件来修改存储引擎配置，修改完成后需要重启Docker才能生效
  ```
  如果修改了正在运行Docker主机的存储引擎类型，则现有的镜像在重启之后将不可用，这是因为每种存储驱动在主机上存储镜像层的位置是不同的(/var/docker/<storage-driver>/...目录下)。修改了存储驱动类型，Docker就无法找到原有的镜像和容器了。切换到原来的存储驱动，之前的镜像和容器就可以继续使用了。若希望在切换存储引擎之后还能够继续使用之前的镜像和容器，需要将镜像保存为Docker格式，上传到某个镜像仓库，修改本地Docker存储引擎
  ```

# Docker Image
- 1 It is a object that includes OS filesystem and applications. 类似于虚拟机模板或将它视为一个Java Class
- 2 镜像实际上等价于未运行的容器
- 3 镜像ID具有唯一性
- 4 镜像查看命令: docker image ls
- 5 镜像拉取命令：

# Docker Container
- 1 A Container is a runnable instance of an Image
- 2 You can create, start, stop, move, or delete a container using Docker API or CLI
- 3 You can connect a container to one or more networks
- 4 You can attach storage to it
- 5 You can create a new image based on its current state
- 6 You can control how isolated a container's network,storage,or other underlying subsystems are from other containers or from the host machine
- 7 Define a container with ``` Dockefile ``` portable images are defined,for example:

    ```
    # Use an official Python runtime as a parent image
    FROM python:2.7-slim

    # Set the working directory to /app
    WORKDIR /app

    # Copy the current directory contents into the container at /app
    COPY . /app

    # Install any needed packages specified in requirements.txt
    RUN pip install --trusted-host pypi.python.org -r requirements.txt

    # Make port 80 available to the world outside this container
    EXPOSE 80

    # Define environment variable
    ENV NAME World

    # Run app.py when the container launches
    CMD ["python", "app.py"]
    ```
# Docker Services
- 1 It allows you to scale containers across multiple Docker daemons,which all work together as a swarm with multiple managers and workers.
- 2 Each member of a SWARM is a Docker daemon, and the daemons all communicate using the DOcker API.
- 3 A service allows you to define the desired state, such as the number of replicas of the service that must be available at any given time.
- 4 By default, the service is load-balanced across all worker nodes.
- 5 To the consumer, the Docker service appears to be a single applicatoin.
- 6 Docker Engine supports swarm mode in Docker 1.12 and higher.

# Docker Hub

# Docker 本章所提及的 Commands
- 1 ``` docker --version ```
- 2 ``` docker version ```
- 3 ``` docker system info ```
- 4 ``` docker images ``` or ``` docker image ls ```
- 5 ``` docker container ls ```
- 6 ``` docker run hello-world ```
