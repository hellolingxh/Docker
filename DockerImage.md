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
- Dokcer 提供命令移除全部的悬虚镜像以及额外没有被使用过的镜像
  - docker image prune
  - docker image prune -a
- docker 目前支持如下的过滤器
  - dangling: true OR false 表示返回悬虚镜像OR非悬虚镜像
  - before: 需要镜像名或ID镜作为参数，返回在之前被创建的全部镜像
  - since: 与before刚好相反
  - label: 根据标注(label)名称或值对镜像进行过滤。因为docker image ls命令输出中不显示label内容
  - reference 过滤
    -　使用reference完成过滤并且仅显示标签为latest的示例
      - docker image ls --filter=reference="*:latest"
  - format参数通过Go模板对输出内容进行格式化
    -　只返回Docker主机上镜像大小属性
      - docker image ls --format "{{.Size}}
    - 返回全部镜像但只显示仓库，标签和大小信息
      - docker image ls --format "{{.Repository}}:{{.Tag}}:{{.Size}}"
  - 需要更复杂的过滤还可以使用OS或Shell自带的工具: Grep, AWK, Sed
  
# 通过CLI方式搜索Docker Hub
- docker search　命令通过CLI的方式搜索Docker Hub
  - 默认情况下，该命令只返回25行结果,但是可以指定--limit参数来增加返回的行数，最多为100行
  - 可以指定　NAME　字段的内容进行匹配,搜索指定NAME值的仓库(包含Docker ID或非官方仓库的组织名称）
    - docker search alpine
  - 也可以基于返回内容中任意列的值进行过滤
    - docker search alpine --filter "is-official=true"
    - docker search alpine --filter "is-automated=true"

# 镜像与分层
- Docker镜像由一些松耦合的只读镜像层组成，Docker负责堆叠这些镜像层，并且将它们表示为单个统一的对象
- 查看和检查构成某个镜像的分层
  - docker image pull ubuntu:latest *镜像ID作为标识*
  - docker image inspect ubuntu:latest *SHA256散列值标识
- 所有的Docker镜像都起始于一个基础镜像层，当修改或增加新内容时，就会在当前镜像层上创建新的镜像层.在添加额外的镜像层的同时，Docker镜像始终保持当前所有镜像层对象的组合
  - 如果该镜像包含2个镜像层，每个镜像层有3个文件，则该镜像拥有2个镜像层的6个文件对象
  - 如果该镜像包含2个镜像层，每个镜像层有３个文件，但第2层有1个文件是第1层的1个文件的更新版本的话，则该docker镜像共有2个镜像层的5个文件对象
- Docker通过存储引擎(新版本采用愉照机制)的方式来实现镜像层的堆栈，并保证多镜像层对外展示为统一的文件系统,
  - Linux上可用的存储引擎如下,每种存储引擎都基于Linux中对应的文件系统或块设备技术，各自独有性能特点
    - AUFS
    - Overlay2
    - Deveice Mapper
    - Btrfs
    - ZFS  
  - Windows上仅支持windowsfilter一种存储引擎，基于NTFS文件系统上实现了分层和CoW
- 

＃　共享镜像层
- 多个镜像之间可以也确实会共享镜像层，这样可以有效节省空间并提升性能
- 当执行镜像拉取命令时，当镜像层显示以Already exists结尾时，说明该镜像已存在于Docker主机

# 根据摘要拉取镜像
- 由于标签是可变的，但摘要信息只要镜像层的内容不发生改变则该信息是永远不会改变
  - docker image pull --digests alpine *查看alpine镜像摘要*
  - docker image pull alpine@sha256:xxxxxxxxxxxx *拉取指定摘要的alpine镜像*

# 镜像散列值(摘要)
- 镜像本身就是一个配置对象，其中包含　了镜像层的列表和一些元数据信息
- 镜像层才是实际数据存储的地方，比如文件等。镜像层之间是完全独立的，并没有从属于某个镜像集合的概念
- 镜像的唯一标识是一个加密的ID，即配置对象本身的散列值。每个镜像层也由一个加密ID区分，其值为镜像层本身内容的散列值
- 所以镜像和其镜像层任何改动都能很轻松地被辨别,这就是所谓的内容散列(Content Hash)
- 在推送和拉取镜像时，都会对镜像层进行压缩来节省网络带宽和仓库二进制存储空间。但压缩会改变镜像内容，将导致内容散列值不同。解决办法:
  - 每个镜像层同时会包含一个分发散列值(Distribution Hash)，该值是一个压缩版镜像散列值，该值会用于校验拉取的镜像是否被篡改
- 内容寻址存储模型极大提升了镜像的安全性，也解决了随机生成镜像和镜像层ID这种方式可能导致的ID冲突问题

# 多层架构的镜像
- Docker(镜像和镜像仓库服务)规范目前支持多架构镜像，这意味着一个镜像标签下可以支持多个平台和架构,为了实现该特性，镜像仓库服务API支持两种重要结构:
  - Manifest List: 列出支持的架构列表,列表中的每一项都含有一个指针，指向对应的Manifest File
  - Manifest File: 出现在架构列表中的平台都有对应的Manifest File, 包含了镜像配置和镜像层数据
  - 在发布镜像时，Manifest List是可选的，若无Manifest List的情况下，镜像仓库服务会返回普通的Manifest
- Multi-architecture Image: 无须考虑镜像是否与当前运行环境的架构匹配
  - 当Docker Client调用Docker Hub镜像仓库服务相应API进行拉取，
  - 若该镜像存在Manifest列表则找到该Manifest File
  - 解析出组成该镜像的镜像层加密ID
  - 最后从Docker Hub二进制存储中拉取每个镜像层
- Docker本身能识别Host的架构或平台，然后自动完成对应架构的镜像拉取

# 删除镜像
- 当不再需要某镜像时，可以通过命令从Docker Host删除该镜像
  - $ docker image rm
  - 该命令会在当前主机上删除该镜像以及相关的镜像层和包含镜像层数据的目录也会被删除
  - 若某个镜像层被多个镜像共享,那只有当全部依赖该镜像层的镜像都被删除后，该镜像层才会被删除
  - 被执行删除的镜像不能有存在的容器正在运行它
  - 删除Host上的所有镜像
    - $ docker image rm $(docker image ls -q) -f
  - Windows环境中，只有在PowerShell终端中才可以执行，CMD中执行不会生效

# 镜像常用命令
- docker image pull
  - 默认会从Docker Hub的仓库中拉取
- docker image ls
  - docker image ls --digests 来查看镜像的SHA256签名
- docker image inspect
  - 查看镜像细节，包括镜像层数据和元数据
- docker image rm
  - 当镜像存在关联的容器，并且容器处于运行(Up)或者停止(Exited)状态时，不允许删除该镜像

  
