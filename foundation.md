# Docker 简介
- 1 是一种运行于Linux和Ｗindows上的软件，用于创建，管理和编排容器。
- 2 开源项目叫做Moby on Github
- 3 主要有两个版本：EE & CE
- 4 Features:Batteries included but removable
- 5 两个主要组件：Docker Client & Docker Daemon. 它们之间的通信是通过本地IPC/UNIX Socket完成的(/var/run/docker.sock)
- 6 查看详细版本信息执行: docker verion
- 7 主要使用Go语言实现

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
- 9 Verify that Docker Engine - Community is installed correctly by running the ``` hello-world ``` image.
  
  ``` $ sudo docker run hello-world ```


## Download the Deb package and install it manually

## Use automated convenience scripts to install Docker in testing and development environment

# Docker 升级 on Ubuntu

# Docker 存储驱动

# Docker Image
- 1 It is a object that includes OS filesystem and applications. 类似于虚拟机模板或将它视为一个Java Class
- 2 镜像实际上等价于未运行的容器
- 3 镜像ID具有唯一性
- 4 镜像查看命令: docker image ls
- 5 镜像拉取命令：

# Docker Container

# Docker Hub
