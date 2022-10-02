# LXD

<figure><img src=".gitbook/assets/containers.png" alt=""><figcaption></figcaption></figure>

LXD 是一个现代、安全且强大的系统容器和虚拟机管理器。

它为在容器或虚拟机内运行和管理的完整 Linux 系统提供了统一的用户体验。LXD 为许多 Linux 发行版提供了[镜像文件](https://images.linuxcontainers.org/)，并构建了一套非常强大且简单的 REST API。LXD 可以从单机上的一个实例扩展到完整的数据中心集群，使其适合运行在开发或生产环境中的服务负载。

LXD 可以让你像私有云一样轻松的搭建一套系统，您可以高效地运行任何类型的服务负载。

如果您想封装不同的环境或运行虚拟机，或者想使用高效且相对经济的方式管理基础架构，那么您可以考虑使用 LXD。

## 开始

为了更好的了解什么是 LXD，可以用来做什么，您可以点击[在线体验](https://linuxcontainers.org/lxd/try-it/)进行尝试。如果您想在本地环境中进行构建，请参阅我们的[入门指南](https://linuxcontainers.org/lxd/getting-started-cli/)。

发布公告：[https://linuxcontainers.org/lxd/news/](https://linuxcontainers.org/lxd/news/)

资源下载：[https://linuxcontainers.org/lxd/downloads/](https://linuxcontainers.org/lxd/downloads/)

在线文档：[https://linuxcontainers.org/lxd/docs/latest/](https://linuxcontainers.org/lxd/docs/latest/)

## 安全性

为了保证您的 LXD 安全安装，请参考以下方式：

* 确保您的操作系统处于最新的状态，并且安装了必要的安全补丁。
* 仅使用能够获取支持的 LXD 版本（LTS 版本或每月更新的功能性版本）。
* 对 LXD 守护进程和远程 API 做好权限控制。
* 非必要情况下，请不要使用有特殊权限的容器，如需使用，请采取适当的安全措施。更多信息，请参阅 [LXC 安全页面](https://linuxcontainers.org/lxc/security/)。
* 请确保您的网络配置是安全的。

更多信息，请前往[安全](zhun-bei-kai-shi/an-quan.md)。

> :fire: 注意
>
> 通过本地 Unix 套接字访问的 LXD 始终被授予完整的访问权限。这包括将文件系统挂载到任何实例的权限，以及调整任何实例上的安全功能。
>
> 因此，您应该将这种访问权限授予您信任的 root 用户。

## 项目和社区

LXD 是一个基于 Apache 2.0 许可的免费项目。这是一个开源项目，十分欢迎大家参与社区的建设和贡献。

LXD 项目由 Canonical 公司赞助。

* [Code of Conduct](https://github.com/lxc/lxd/blob/master/CODE\_OF\_CONDUCT.md)
* [提交贡献到项目](zhun-bei-kai-shi/gong-xian.md)
* [获取支持](zhun-bei-kai-shi/zhi-chi.md)
* [观看教程和公告](https://www.youtube.com/c/LXDvideos)
* [在 IRC 上进行讨论](https://web.libera.chat/#lxc)
* [在论坛中进行讨论](https://discuss.linuxcontainers.org/)
* [邮件列表](https://lists.linuxcontainers.org/)

