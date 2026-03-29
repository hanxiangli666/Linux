# Creating Your Own SYSTEMD Service

# 创建你自己的 SYSTEMD 服务

- Take me to the [Tutorial](https://kodekloud.com/topic/creating-a-systemd-service/)
- 前往 [教程视频](https://kodekloud.com/topic/creating-a-systemd-service/)

---

In this lecture we will learn how to create a SYSTEMD Service. SYSTEMD is the de facto standard init system on modern Linux distributions, and understanding how to create custom service units is an essential skill for any Linux administrator or developer deploying applications.

本讲将学习如何创建 SYSTEMD 服务。SYSTEMD 是现代 Linux 发行版事实上的标准初始化系统，掌握如何创建自定义服务单元是任何 Linux 管理员或应用部署开发者必备的核心技能。

- All major distributions, such as RHEL, CentOS, Fedora, Ubuntu, Debian, and Arch Linux, have adopted systemd as their init system.
- 所有主流发行版，如 RHEL、CentOS、Fedora、Ubuntu、Debian 和 Arch Linux，都已采用 systemd 作为其初始化系统。

- Systemd is a Linux initialization system and service manager that includes features like on-demand starting of daemons, mount and automount point maintenance, dependency-based service activation, and more.
- Systemd 是一个 Linux 初始化系统和服务管理器，具备按需启动守护进程、维护挂载点和自动挂载点、基于依赖关系的服务激活等众多特性。

- Systemd also provides a logging daemon (`journald`) and other tools and utilities to help with common system administration tasks.
- Systemd 还提供了日志守护进程（`journald`）以及其他工具和实用程序，帮助完成常见的系统管理任务。

---

## What is a Service Unit?

## 什么是服务单元（Service Unit）？

A file with the `.service` suffix contains information about a process which is managed by systemd. These files are typically located in one of the following directories:

带有 `.service` 后缀的文件包含由 systemd 管理的进程相关信息。这些文件通常位于以下目录之一：

| Directory | Purpose |
|-----------|---------|
| `/lib/systemd/system/` | Default units shipped with packages / 软件包自带的默认单元 |
| `/etc/systemd/system/` | Custom units created by the administrator / 管理员创建的自定义单元（优先级更高） |
| `/run/systemd/system/` | Runtime units, non-persistent / 运行时单元，非持久化 |

A `.service` file is composed of three main sections:

一个 `.service` 文件由三个主要部分组成：

---

### 1. `[Unit]` Section — 单元描述与依赖

The **`[Unit]`** section contains metadata about the service itself and defines its relationships with other units. It is not specific to service units — it is common to all unit types.

**`[Unit]`** 部分包含服务本身的元数据，并定义其与其他单元的关系。它并非服务单元专属，而是所有单元类型的通用部分。

Key options in this section:

该部分的关键选项：

| Option | Description |
|--------|-------------|
| `Description` | A human-readable name shown in `systemctl status` and logs / 在 `systemctl status` 和日志中显示的可读名称 |
| `Documentation` | Links to documentation (man pages, URLs) / 文档链接（man 手册页、URL） |
| `After` | Ensures this unit starts **after** the listed units / 确保此单元在列出的单元**之后**启动 |
| `Requires` | Hard dependency — if the required unit fails, this unit also fails / 强依赖——若依赖单元失败，本单元也会失败 |
| `Wants` | Soft dependency — starts the listed units but does not fail if they are unavailable / 弱依赖——尝试启动列出的单元，但不强制要求其成功 |

**Example / 示例：**

```ini
[Unit]
Description=Python Django for Project Mercury
Documentation=http://wiki.caleston-dev.ca/mercury
After=postgresql.service
Wants=network-online.target
```

> **Note / 注意：** `After` controls **ordering** only, not dependency. To also require a service, combine it with `Requires` or `Wants`.
>
> `After` 仅控制**启动顺序**，不建立依赖关系。若同时需要某服务，应配合 `Requires` 或 `Wants` 使用。

---

### 2. `[Service]` Section — 服务行为配置

The **`[Service]`** section is specific to `.service` units. It defines how the service is started, stopped, and managed.

**`[Service]`** 部分是 `.service` 单元专属的配置区段，定义了服务的启动、停止和管理方式。

Key options in this section:

该部分的关键选项：

| Option | Description |
|--------|-------------|
| `ExecStart` | The command to run when the service starts / 服务启动时执行的命令 |
| `ExecStop` | The command to run when the service stops (optional) / 服务停止时执行的命令（可选） |
| `ExecReload` | The command to reload the service configuration / 重新加载服务配置的命令 |
| `User` | The user account under which the service runs / 服务运行所使用的用户账户 |
| `WorkingDirectory` | The working directory for the process / 进程的工作目录 |
| `Restart` | When to automatically restart (`always`, `on-failure`, `on-abnormal`, etc.) / 何时自动重启（`always`、`on-failure`、`on-abnormal` 等） |
| `RestartSec` | Time to wait before restarting / 重启前等待的时间 |
| `Type` | Service type (`simple`, `forking`, `oneshot`, `notify`, etc.) / 服务类型（`simple`、`forking`、`oneshot`、`notify` 等） |
| `Environment` | Set environment variables for the service / 为服务设置环境变量 |
| `EnvironmentFile` | Load environment variables from a file / 从文件加载环境变量 |

**Example / 示例：**

```ini
[Service]
ExecStart=/usr/bin/project-mercury.sh
User=project_mercury
WorkingDirectory=/opt/project-mercury
Restart=on-failure
RestartSec=10
Environment="DEBUG=false"
```

> **Tip / 提示：** Setting `Restart=on-failure` means systemd will automatically restart your service if it crashes, greatly improving application availability.
>
> 设置 `Restart=on-failure` 意味着当服务崩溃时，systemd 会自动重启它，大幅提升应用的可用性。

---

### 3. `[Install]` Section — 安装与启用配置

The **`[Install]`** section defines how and when the service should be enabled (i.e., at which boot target it should be activated).

**`[Install]`** 部分定义了服务应如何以及何时被启用（即在哪个启动目标下激活）。

Key options in this section:

该部分的关键选项：

| Option | Description |
|--------|-------------|
| `WantedBy` | The target that wants this service — most commonly `multi-user.target` or `graphical.target` / 需要此服务的目标，最常用的是 `multi-user.target` 或 `graphical.target` |
| `RequiredBy` | Similar to `WantedBy` but creates a hard dependency / 类似 `WantedBy`，但建立强依赖 |
| `Alias` | Additional names for this unit / 该单元的别名 |

**Example / 示例：**

```ini
[Install]
WantedBy=multi-user.target
```

> **Note / 说明：** `multi-user.target` is roughly equivalent to the traditional **runlevel 3** (multi-user, non-graphical). `graphical.target` is equivalent to **runlevel 5** (with a desktop environment).
>
> `multi-user.target` 大致相当于传统的**运行级别 3**（多用户、无图形界面）。`graphical.target` 相当于**运行级别 5**（包含桌面环境）。

---

## Complete Example: Project Mercury Service

## 完整示例：Project Mercury 服务

Here is a complete, well-commented example service file:

以下是一个完整的、带注释的服务文件示例：

```ini
# /etc/systemd/system/project-mercury.service

[Unit]
Description=Python Django for Project Mercury
Documentation=http://wiki.caleston-dev.ca/mercury
After=postgresql.service network-online.target
Wants=network-online.target

[Service]
User=project_mercury
WorkingDirectory=/opt/project-mercury
ExecStart=/usr/bin/project-mercury.sh
Restart=on-failure
RestartSec=10
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

To view the file you created:

查看创建好的文件：

```bash
[~]$ cat /etc/systemd/system/project-mercury.service
```

---

## How to Start the Service

## 如何启动服务

After creating or modifying a unit file, systemd must be notified of the changes. Then you can start and enable the service:

创建或修改单元文件后，必须通知 systemd 感知这些变化。之后就可以启动并启用服务：

```bash
# Reload systemd to discover new/changed unit files
# 重新加载 systemd 以发现新增或变更的单元文件
[~]$ systemctl daemon-reload

# Start the service immediately
# 立即启动服务
[~]$ systemctl start project-mercury.service

# Enable the service to start automatically at boot
# 启用服务，使其在系统启动时自动运行
[~]$ systemctl enable project-mercury.service

# Verify the service is running
# 验证服务是否正在运行
[~]$ systemctl status project-mercury.service
```

> **Best Practice / 最佳实践：** Always run `systemctl daemon-reload` after manually editing a unit file. Otherwise, systemd will continue using the cached version of the file.
>
> 手动编辑单元文件后，始终运行 `systemctl daemon-reload`。否则 systemd 将继续使用文件的缓存版本，变更不会生效。

---

## Summary

## 小结

| Step | Command | Purpose |
|------|---------|---------|
| 1 | Create `/etc/systemd/system/myapp.service` | Define the service unit / 定义服务单元 |
| 2 | `systemctl daemon-reload` | Reload systemd configuration / 重新加载 systemd 配置 |
| 3 | `systemctl start myapp.service` | Start the service now / 立即启动服务 |
| 4 | `systemctl enable myapp.service` | Enable auto-start at boot / 设置开机自动启动 |
| 5 | `systemctl status myapp.service` | Verify the service is running / 验证服务运行状态 |
