# 要求

## Go

LXD 需要 Go 1.18 或以上版本，并且只能使用 Go 编译器进行测试。

我们建议至少提供 2GB RAM 以完成构建。

## 内核

最低支持 5.4 内核版本。

LXD 需要内核支持：

* Namespaces（`pid`, `net`, `uts`, `ipc`, `mount` )&#x20;
* Seccomp
* Native Linux AIO ([`io_setup(2)`](https://man7.org/linux/man-pages/man2/io\_setup.2.html), etc.)

以下的可选功能还需要额外的内核支持：

* Namespaces (`user` and `cgroup`)
* AppArmor （包含可挂载的 Ubuntu 介质补丁）
* Control Groups (`blkio`, `cpuset`, `devices` , `memory`, `pids` and `net_prio`)
* CRIU （包括 CRIU upstream 的一些细节）

以及正在使用的 LXC 版本所需的任何其他内核特性。

## LXC

LXD 需要具有以下构建选项的 LXC 4.0.0 或更高版本：

* `apparmor` (如果使用 LXD AppArmor)
* `seccomp`

要运行各种发行版的最新版本，包括Ubuntu，还应安装 LXCFS。

## QEMU

虚拟机需要 QEMU 6.0 或更高版本支持。

## 其他库 (头文件)

LXD 使用 `dqlite` 作为数据库， 可以运行 `make deps` 进行构建或设置,。

LXD本身也使用了许多（通常是打包的）C库：

* `libacl1`
* `libcap2`
* `liblz4` (for `dqlite`)
* `libuv1` (for `dqlite`)
* `libsqlite3` >= 3.25.0 (for `dqlite`)

确保安装了所有这些库本身及其头文件（-dev包）。
