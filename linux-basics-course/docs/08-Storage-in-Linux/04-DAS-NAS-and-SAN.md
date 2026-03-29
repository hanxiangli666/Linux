
# DAS, NAS, and SAN
# 直连存储、网络附加存储与存储区域网络

> This section covers the three major external storage architectures used in enterprise environments: DAS, NAS, and SAN — including how to configure NFS on Linux.
>
> 本节介绍企业环境中三种主要的外部存储架构：DAS、NAS 和 SAN，并包含如何在 Linux 上配置 NFS。

- Take me to the [Tutorial](https://kodekloud.com/topic/das-nas-and-san/)

---

## Overview
## 概述

Now that you are familiar with the basics of storage in Linux, let's learn about **external storage** solutions. In enterprise environments, storage is rarely confined to a single machine — it is shared, centralized, and managed at scale.

了解了 Linux 存储基础知识之后，让我们来学习**外部存储**解决方案。在企业环境中，存储很少局限于单台机器，而是以共享、集中和规模化的方式进行管理。

---

## DAS — Direct Attached Storage
## DAS — 直连存储

**DAS** is the simplest form of external storage. The storage device (e.g., an external hard drive, a RAID array, a tape drive) is connected **directly** to the host computer via interfaces such as SATA, SAS, USB, or Thunderbolt.

**DAS** 是最简单的外部存储形式。存储设备（如外置硬盘、RAID 阵列、磁带驱动器）通过 SATA、SAS、USB 或 Thunderbolt 等接口**直接**连接到主机。

**Characteristics / 特点：**
- Fast and low-latency, as there is no network overhead. / 速度快、延迟低，无网络开销。
- Only accessible by the directly connected host. / 只能由直连的主机访问。
- Simple to set up and manage. / 设置和管理简单。
- Not easily shared across multiple servers. / 不易在多台服务器之间共享。

**Typical use cases / 典型使用场景：** Desktop workstations, single-server environments, development machines.
**典型应用：** 台式工作站、单服务器环境、开发机器。

---

## NAS — Network Attached Storage
## NAS — 网络附加存储

**NAS** is a file-level storage server connected to a network, allowing multiple clients to access shared storage over standard network protocols like NFS (Unix/Linux) or SMB/CIFS (Windows).

**NAS** 是一种连接到网络的文件级存储服务器，允许多个客户端通过 NFS（Unix/Linux）或 SMB/CIFS（Windows）等标准网络协议访问共享存储。

**Characteristics / 特点：**
- Accessible by multiple clients simultaneously. / 可同时被多个客户端访问。
- Uses **file-level** protocols — clients see a directory tree, not raw blocks. / 使用**文件级**协议——客户端看到的是目录树，而非原始数据块。
- Easy to expand capacity without downtime. / 易于在不停机的情况下扩展容量。
- Performance depends on network speed. / 性能受网络速度限制。

**Typical use cases / 典型应用：** Shared file storage, home directories, media servers, backup targets.
**典型应用：** 共享文件存储、用户主目录、媒体服务器、备份目标。

---

## SAN — Storage Area Network
## SAN — 存储区域网络

**SAN** is a high-speed, dedicated network that provides **block-level** access to storage. Hosts connect to a SAN via **Fiber Channel (FC)** or **iSCSI** and see the storage as if it were a locally attached disk.

**SAN** 是一种高速专用网络，提供对存储的**块级**访问。主机通过**光纤通道（FC）**或 **iSCSI** 连接到 SAN，并将存储视为本地附加磁盘。

**Characteristics / 特点：**
- Extremely high performance and low latency (especially with Fiber Channel). / 极高的性能和极低的延迟（尤其是使用光纤通道时）。
- Block-level access — the host manages its own file system on top of the SAN volume. / 块级访问——主机在 SAN 卷之上管理自己的文件系统。
- More complex and expensive to deploy. / 部署更复杂、成本更高。
- Ideal for mission-critical applications like databases. / 非常适合数据库等关键任务应用程序。

**Typical use cases / 典型应用：** Enterprise databases (Oracle, SQL Server), virtualization platforms (VMware), high-performance computing.
**典型应用：** 企业数据库（Oracle、SQL Server）、虚拟化平台（VMware）、高性能计算。

---

## Comparison Table
## 对比表格

| Feature | DAS | NAS | SAN |
|---------|-----|-----|-----|
| Access type | Block | File | Block |
| 访问类型 | 块级 | 文件级 | 块级 |
| Network required | No | Yes (LAN) | Yes (dedicated FC/iSCSI) |
| 是否需要网络 | 否 | 是（局域网）| 是（专用 FC/iSCSI）|
| Shared access | No | Yes | Yes |
| 共享访问 | 否 | 是 | 是 |
| Performance | High | Medium | Very High |
| 性能 | 高 | 中 | 极高 |
| Cost | Low | Medium | High |
| 成本 | 低 | 中 | 高 |
| Complexity | Low | Medium | High |
| 复杂度 | 低 | 中 | 高 |

---

## NFS — Network File System
## NFS — 网络文件系统

**NFS (Network File System)** is the standard Unix/Linux protocol for sharing directories over a network. Unlike block-based protocols, NFS saves and transfers data in the form of **files**, operating on a **server-client** model.

**NFS（网络文件系统）**是 Unix/Linux 系统中通过网络共享目录的标准协议。与基于块的协议不同，NFS 以**文件**的形式保存和传输数据，采用**服务器-客户端**模型运行。

### Server Side — Configuring Exports
### 服务器端——配置共享目录

The NFS server maintains an export configuration file at `/etc/exports` that defines which directories are shared and which clients are permitted to access them:

NFS 服务器在 `/etc/exports` 中维护一个共享配置文件，定义哪些目录被共享以及哪些客户端被允许访问：

```bash
# /etc/exports 示例 / Example /etc/exports
# 格式：<目录>  <客户端IP或主机名>(<选项>)
# Format: <directory>  <client-ip-or-hostname>(<options>)

/software/repos  10.61.35.201(rw,sync,no_subtree_check) \
                 10.61.35.202(rw,sync,no_subtree_check) \
                 10.61.35.203(ro,sync,no_subtree_check)
```

Common export options / 常用导出选项：

| Option | Meaning | 选项 | 含义 |
|--------|---------|------|------|
| `rw` | Read-write access | `rw` | 读写访问 |
| `ro` | Read-only access | `ro` | 只读访问 |
| `sync` | Write data to disk before replying | `sync` | 回复前先将数据写入磁盘 |
| `no_subtree_check` | Disable subtree checking (improves reliability) | `no_subtree_check` | 禁用子树检查（提高可靠性）|
| `no_root_squash` | Allow root on client to have root privileges on server | `no_root_squash` | 允许客户端 root 拥有服务器上的 root 权限 |

### Exporting the Shares
### 导出共享目录

To export all mounts defined in `/etc/exports`:

导出 `/etc/exports` 中定义的所有共享目录：

```bash
[~]$ sudo exportfs -a
```

To manually export a specific directory to a specific client without editing `/etc/exports`:

手动将特定目录导出给特定客户端（无需编辑 `/etc/exports`）：

```bash
[~]$ sudo exportfs -o 10.61.35.201:/software/repos
```

To verify active exports:

查看当前活跃的导出：

```bash
[~]$ sudo exportfs -v
/software/repos 10.61.35.201(sync,wdelay,hide,no_subtree_check,sec=sys,rw,secure,root_squash,no_all_squash)
```

### Client Side — Mounting an NFS Share
### 客户端——挂载 NFS 共享

```bash
# 在客户端创建挂载点 / Create a mount point on the client
[~]$ sudo mkdir -p /mnt/software/repos

# 挂载 NFS 共享 / Mount the NFS share
[~]$ sudo mount -t nfs 10.61.35.201:/software/repos /mnt/software/repos

# 验证挂载 / Verify the mount
[~]$ df -hP | grep repos
10.61.35.201:/software/repos  100G  20G  80G  20% /mnt/software/repos
```

To make the NFS mount persistent, add it to `/etc/fstab`:

要使 NFS 挂载持久化，将其添加到 `/etc/fstab`：

```bash
# /etc/fstab 条目 / /etc/fstab entry
10.61.35.201:/software/repos  /mnt/software/repos  nfs  defaults,_netdev  0  0
```

> The `_netdev` option tells the system to wait for the network to be available before attempting to mount — essential for network-based file systems.
>
> `_netdev` 选项告诉系统在尝试挂载之前等待网络就绪——这对基于网络的文件系统至关重要。

---

# Hands-On Labs
# 动手实验

- [NFS Labs](https://kodekloud.com/courses/873064/lectures/17311763) — practice setting up an NFS server and mounting shares from a client.
- [NFS 动手实验](https://kodekloud.com/courses/873064/lectures/17311763) — 练习搭建 NFS 服务器并从客户端挂载共享目录。
