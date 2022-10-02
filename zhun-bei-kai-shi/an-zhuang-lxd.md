# 安装 LXD

安装LXD的最简单方法是安装一个可用的软件包，但您也可以从源安装 LXD。

## 通过安装包

LXD 守护进程仅在 Linux 上工作，但客户端工具（LXC）在大多数平台上可用。

| OS      | FORMAT                                            | COMMAND             |
| ------- | ------------------------------------------------- | ------------------- |
| Linux   | [Snap](https://snapcraft.io/lxd)                  | `snap install lxd`  |
| Windows | [Chocolatey](https://chocolatey.org/packages/lxc) | `choco install lxc` |
| macOS   | [Homebrew](https://formulae.brew.sh/formula/lxc)  | `brew install lxc`  |

为各种 Linux 发行版和操作系统安装 LXD 的更多说明，请访问[我们的网站](https://linuxcontainers.org/lxd/getting-started-cli/)。

## 通过源安装

我们建议为 LXD 开发提供最新版本的 liblxc（需要>=4.0.0）。此外，LXD 需要 Go 1.18 或更高版本才能工作。在 Ubuntu 上，您可以通过以下方式获得：

```bash
sudo apt update
sudo apt install acl attr autoconf automake dnsmasq-base git golang libacl1-dev libcap-dev liblxc1 liblxc-dev libsqlite3-dev libtool libudev-dev liblz4-dev libuv1-dev make pkg-config rsync squashfs-tools tar tcl xz-utils ebtables
```

除了默认的 dir 驱动程序外，还有一些用于 LXD 的存储驱动程序。安装这些工具会给 initramfs 添加一些内容，但这可能会降低主机启动速度，如果您想使用特定的驱动程序，则需要使用这些工具：

```bash
sudo apt install lvm2 thin-provisioning-tools
sudo apt install btrfs-progs
```

要运行测试套件，还需要：

```bash
sudo apt install curl gettext jq sqlite3 socat bind9-dnsutils
```

### 通过源安装：编译最新版本

这些从源代码构建的说明适用于个人开发人员，可以通过这种方式，构建最新版本的 LXD ，或者构建 Linux 发行版中可能不提供的特定 LXD 的情况。此处不介绍用于集成到 Linux 发行版的源代码构建，将来可能会在单独的文档中详细介绍。

```bash
git clone https://github.com/lxc/lxd
cd lxd
```

这将下载 LXD 的当前开发分支，接下来按照下面的说明实际构建和安装 LXD。

### 通过源安装：构建和发布

LXD 发行版捆绑了一个完整的依赖树，以及用于 LXD 数据库设置的 libraft 和 libdqlite 的本地副本。

```bash
tar zxvf lxd-4.18.tar.gz
cd lxd-4.18
```

这将解压缩 Release 文件，然后按照下面的说明实际构建和安装LXD。

### 开始构建

构建实际上是通过调用两个单独的 Makefile 完成的：`make` `deps`（构建 LXD 所需的库）和 `make`（构建 LXD 本身）。在 `make deps` 结束后，将显示一条消息，指定调用 `make` 之前应该设置的环境变量。随着新版本的 LXD 的发布，这些环境变量设置可能会发生变化，因此请确保使用 `make deps`过程结束时显示的设置，因为下面的设置（示例）可能与您的版本 LXD 要求不完全匹配：

我们建议至少有 2GB 的 RAM 以完成构建。

```bash
make deps
# 按照 make deps 中的说明导出所需的环境变量。
# 例如:
#  export CGO_CFLAGS="${CGO_CFLAGS} -I$(go env GOPATH)/deps/dqlite/include/ -I$(go env GOPATH)/deps/raft/include/"
#  export CGO_LDFLAGS="${CGO_LDFLAGS} -L$(go env GOPATH)/deps/dqlite/.libs/ -L$(go env GOPATH)/deps/raft/.libs/"
#  export LD_LIBRARY_PATH="$(go env GOPATH)/deps/dqlite/.libs/:$(go env GOPATH)/deps/raft/.libs/:${LD_LIBRARY_PATH}"
#  export CGO_LDFLAGS_ALLOW="(-Wl,-wrap,pthread_create)|(-Wl,-z,now)"
make
```

### 通过源安装：开始安装

构建完成后，只需保留源代码分支，将 `$(go env GOPATH)/bin` 引用的目录添加到 shell 路径，并将 `make deps` 打印的 `LD_LIBRARY_PATH` 变量设置到您的环境中。对于 `~/bashrc`来说，这可能看起来像这样：

```bash
export PATH="${PATH}:$(go env GOPATH)/bin"
export LD_LIBRARY_PATH="$(go env GOPATH)/deps/dqlite/.libs/:$(go env GOPATH)/deps/raft/.libs/:${LD_LIBRARY_PATH}"
```

现在，您可以使用 `lxd` 和 `lxc` 二进制文件，并可以用来启动 LXD。由于`LD_LIBRARY_PATH` 环境变量，二进制文件将自动查找并使用 `$(go env GOPATH)/deps` 中内置的依赖项。

### 设置

root 需要 sub {u,g} ids，这样 LXD 才能创建无特权的容器：

```bash
echo "root:1000000:1000000000" | sudo tee -a /etc/subuid /etc/subgid
```

现在您可以运行守护程序（ `--group sudo` 允许 `sudo` 组中的每个人与 LXD 通信；如果需要，您可以创建自己的组）：

```bash
sudo -E PATH=${PATH} LD_LIBRARY_PATH=${LD_LIBRARY_PATH} $(go env GOPATH)/bin/lxd --group sudo
```

> :pencil2: 笔记
>
> 如果系统上存在 `newuidmap/newgidmap` 工具，并且 `/etc/subuid`，`etc/subid` 存在，则必须将其配置为允许 root 用户具有至少 10M UID/GID 的连续范围。

## 升级 LXD

将 LXD 升级到新版本后，LXD 可能需要将其数据库更新为新对象。当 LXD 升级后启动守护程序时，此更新会自动进行。更新之前的数据库备份存储在与当前数据库相同的位置（例如，对于快照安装，存储在 `/var/snap/lxd/common/lxd/database`）。

> :fire: 注意
>
> 数据库更新后，旧版本的 LXD 数据库可能变为无效。这意味着降级 LXD 可能会导致您的 LXD 安装失败。
>
> 在这种情况下，如果需要降级，请在开始降级之前恢复数据库备份。
