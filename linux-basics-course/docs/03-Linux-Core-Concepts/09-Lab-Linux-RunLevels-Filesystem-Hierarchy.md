# Lab - Linux Runlevels and Filesystem Hierarchy
# 实验 - Linux 运行级别与文件系统层级结构

- Access Hands-On Labs here [Hands-On Labs](https://kodekloud.com/topic/lab-linux-kernel-modules-boot-and-filetypes/)

This lab covers practical exercises on systemd targets (runlevels), file types, filesystem hierarchy, and hardware information. Each exercise includes the command, detailed explanation, and additional context.

本实验涵盖 systemd 目标（运行级别）、文件类型、文件系统层级结构和硬件信息的实践练习。每道题都包含命令、详细解释以及扩展内容。

---

### Exercise 1 — Run a Command with sudo Privileges
### 练习 1 — 使用 sudo 权限运行命令

Some commands require **root (superuser) privileges** to execute. Use `sudo` to run them with elevated permissions.

某些命令需要 **root（超级用户）权限**才能执行。使用 `sudo` 以提升的权限运行它们。

```bash
$ sudo ls /root
Desktop  Documents  Downloads  snap
```

**Explanation / 说明**: `/root` is the home directory of the root user. Regular users cannot list its contents — only root (or users with sudo access) can.

`/root` 是 root 用户的家目录。普通用户无法列出其内容——只有 root（或具有 sudo 访问权限的用户）才能访问。

```bash
# Without sudo — permission denied / 不用 sudo — 权限被拒
$ ls /root
ls: cannot open directory '/root': Permission denied

# With sudo — works / 加 sudo — 成功
$ sudo ls /root
```

> **How sudo works / sudo 的工作原理**: When you run `sudo <command>`, the system checks `/etc/sudoers` to see if your user is allowed to run that command as root. If yes, it prompts for **your own password** (not root's password), then executes the command as root.
>
> 运行 `sudo <command>` 时，系统检查 `/etc/sudoers` 以确认你的用户是否被允许以 root 身份运行该命令。如果允许，系统提示输入**你自己的密码**（不是 root 的密码），然后以 root 身份执行命令。

---

### Exercise 2 — Check the Init Process
### 练习 2 — 检查 Init 进程

Determine whether the system uses **systemd** or **SysV init** as its init process.

确定系统使用 **systemd** 还是 **SysV init** 作为其 init 进程。

```bash
$ sudo ls -l /sbin/init
lrwxrwxrwx 1 root root 20 Jan 14 10:00 /sbin/init -> /lib/systemd/systemd
```

**Explanation / 说明**:
- If the output shows `/sbin/init -> /lib/systemd/systemd` (a symlink), the system uses **systemd** / 如果输出显示符号链接指向 systemd，则系统使用 **systemd**
- If `/sbin/init` is a real binary (no symlink arrow), the system likely uses **SysV init** / 如果是真实的二进制文件（无符号链接箭头），则系统可能使用 **SysV init**

```bash
# Additional ways to check / 其他检查方法
$ ps -p 1 -o comm=
systemd

$ systemctl --version
systemd 237

$ file /sbin/init
/sbin/init: symbolic link to /lib/systemd/systemd
```

---

### Exercise 3 — Check the Default systemd Target
### 练习 3 — 检查默认 systemd 目标

Check what **default target** (equivalent to runlevel) the system boots into.

检查系统引导进入的**默认目标**（等同于运行级别）。

```bash
$ sudo systemctl get-default
graphical.target
```

**Or without sudo (also works) / 或不用 sudo（也有效）:**
```bash
$ systemctl get-default
graphical.target
```

**Explanation / 说明**:
- `graphical.target` → System boots to a GUI login screen (Runlevel 5 equivalent) / 系统引导到图形界面登录屏幕（等同于运行级别 5）
- `multi-user.target` → System boots to command line only (Runlevel 3 equivalent) / 系统仅引导到命令行（等同于运行级别 3）

```bash
# Verify by checking the symlink directly / 通过直接检查符号链接来验证
$ ls -l /etc/systemd/system/default.target
lrwxrwxrwx 1 root root 36 Jan 1 00:00 default.target ->
  /lib/systemd/system/graphical.target
```

---

### Exercise 4 — Change the systemd Target to multi-user
### 练习 4 — 将 systemd 目标更改为 multi-user

Switch the default boot target from `graphical.target` to `multi-user.target` (disable GUI on boot).

将默认引导目标从 `graphical.target` 切换到 `multi-user.target`（禁用引导时的图形界面）。

```bash
$ sudo systemctl set-default multi-user.target
Created symlink /etc/systemd/system/default.target →
  /lib/systemd/system/multi-user.target.
```

**Verify the change / 验证更改:**
```bash
$ systemctl get-default
multi-user.target
```

**Switch back to graphical / 切换回图形界面:**
```bash
$ sudo systemctl set-default graphical.target
```

> **Important / 重要**: `set-default` changes what happens on the **next reboot**. To immediately switch without rebooting, use `isolate`:
> ```bash
> # Immediately go to multi-user (stops GUI) / 立即切换到多用户模式（停止图形界面）
> $ sudo systemctl isolate multi-user.target
> ```
>
> `set-default` 更改的是**下次重启**时的行为。要立即切换而不重启，使用 `isolate`。

---

### Exercise 5 — Check the File Type of a .deb File
### 练习 5 — 检查 .deb 文件的类型

Determine the file type of `firefox.deb` located at `/root`.

确定位于 `/root` 的 `firefox.deb` 文件的类型。

```bash
$ sudo file /root/firefox.deb
/root/firefox.deb: Debian binary package (format 2.0), with control.tar.xz, ...
```

**Explanation / 说明**: The `file` command examines the **file's contents** (magic bytes at the beginning of the file) to determine its type — not the extension. This is more reliable than relying on file extensions.

`file` 命令通过检查**文件内容**（文件开头的魔术字节）来确定其类型，而不是依靠扩展名。这比依赖文件扩展名更可靠。

```bash
# Verify it's a regular file in terms of filesystem type / 从文件系统类型角度验证这是普通文件
$ sudo ls -l /root/firefox.deb
-rw-r--r-- 1 root root 76543210 Jan 1 00:00 /root/firefox.deb
│
└── '-' = regular file / '-' = 普通文件

# The 'file' command gives more detailed content type / file 命令给出更详细的内容类型
```

---

### Exercise 6 — Check the File Type of a Shell Script
### 练习 6 — 检查 Shell 脚本的文件类型

Determine the file type of `sample_script.sh` located at `/root`.

确定位于 `/root` 的 `sample_script.sh` 文件的类型。

```bash
$ sudo file /root/sample_script.sh
/root/sample_script.sh: Bourne-Again shell script, ASCII text executable
```

**Explanation / 说明**: The `file` command reads the **shebang line** at the top of the script to determine it's a Bash script:

`file` 命令读取脚本顶部的 **shebang 行**来确定它是 Bash 脚本：

```bash
# The shebang line tells the kernel which interpreter to use / shebang 行告诉内核使用哪个解释器
#!/bin/bash
echo "Hello World"
```

> **What is a shebang? / 什么是 shebang？**: The `#!` at the start of a script (called a "shebang" or "hashbang") tells the kernel which interpreter to use to execute the script. Common examples:
> - `#!/bin/bash` — Bash script / Bash 脚本
> - `#!/usr/bin/python3` — Python 3 script / Python 3 脚本
> - `#!/usr/bin/env node` — Node.js script / Node.js 脚本
>
> 脚本开头的 `#!`（称为"shebang"或"hashbang"）告诉内核使用哪个解释器来执行脚本。

---

### Exercise 7 — Find the Correct Directory for Third-Party Software
### 练习 7 — 找到第三方软件的正确安装目录

**Question / 问题**: You were asked to install a new **third-party IDE** on the system. Which directory is the recommended choice for the installation?

**Answer / 答案**: **`/opt`**

```bash
# Create a subdirectory for the IDE under /opt / 在 /opt 下为 IDE 创建子目录
$ sudo mkdir /opt/my-ide
$ sudo cp -r /path/to/my-ide/* /opt/my-ide/

# Optionally create a symlink in /usr/local/bin for easy access
# 可选：在 /usr/local/bin 中创建符号链接以便访问
$ sudo ln -s /opt/my-ide/bin/my-ide /usr/local/bin/my-ide
```

**Why `/opt` and not other directories? / 为什么是 `/opt` 而不是其他目录？**

| Directory / 目录 | Why NOT / 为什么不适合 |
|---|---|
| `/usr/bin` | Reserved for distribution-managed packages / 保留给发行版管理的包 |
| `/usr/local/bin` | OK for individual binaries, not full applications / 适合单个二进制文件，不适合完整应用 |
| `/home/user` | Not accessible to other users / 其他用户无法访问 |
| `/tmp` | Cleared on reboot / 重启时清除 |
| `/opt` | **Designed exactly for this purpose** / **正是为此设计的** |

---

### Exercise 8 — Find the Directory for Block Device Files
### 练习 8 — 找到块设备文件所在目录

**Question / 问题**: Which directory contains the files related to the block devices seen when running `lsblk`?

**Answer / 答案**: **`/dev`**

```bash
$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0   50G  0 disk
├─sda1   8:1    0   49G  0 part /
└─sda5   8:5    0  975M  0 part [SWAP]

# The device files are in /dev / 设备文件在 /dev 中
$ ls -l /dev/sda*
brw-rw---- 1 root disk 8, 0 Mar 28 /dev/sda
brw-rw---- 1 root disk 8, 1 Mar 28 /dev/sda1
brw-rw---- 1 root disk 8, 5 Mar 28 /dev/sda5
#│
#└── 'b' = block device file / 'b' = 块设备文件
```

**Explanation / 说明**: For every block device listed by `lsblk`, there is a corresponding **block device file** in `/dev/`. The `b` at the start of the permissions string confirms these are block device files.

对于 `lsblk` 列出的每个块设备，`/dev/` 中都有对应的**块设备文件**。权限字符串开头的 `b` 确认这些是块设备文件。

---

### Exercise 9 — Find the Ethernet Controller Vendor
### 练习 9 — 查找以太网控制器供应商

**Question / 问题**: What is the name of the **vendor** for the Ethernet Controller used in this system?

```bash
$ sudo lshw
...
  *-network
       description: Ethernet interface
       product: 82540EM Gigabit Ethernet Controller
       vendor: Intel Corporation
       physical id: 3
       bus info: pci@0000:00:03.0
       logical name: eth0
       ...
```

**Answer / 答案**: Look for the `*-network` section in `lshw` output and find the `vendor` field. In this example: **`Intel Corporation`**

在 `lshw` 输出的 `*-network` 部分找到 `vendor` 字段。本例中为：**`Intel Corporation`**

**Alternative approaches / 替代方法:**
```bash
# Filter to show only network class / 只显示网络类别
$ sudo lshw -class network

# Use lspci to find network controllers / 用 lspci 查找网络控制器
$ lspci | grep -i ethernet
00:03.0 Ethernet controller: Intel Corporation 82540EM Gigabit Ethernet Controller (rev 02)

# Get detailed vendor info with lspci -v / 用 lspci -v 获取详细供应商信息
$ lspci -v | grep -A5 Ethernet
```

---

## Summary — Lab Command Reference
## 小结 — 实验命令参考

| Task / 任务 | Command / 命令 |
|---|---|
| Run command as root / 以 root 身份运行命令 | `sudo <command>` |
| Check init process type / 检查 init 进程类型 | `sudo ls -l /sbin/init` |
| Check default target / 检查默认目标 | `systemctl get-default` |
| Set default target / 设置默认目标 | `sudo systemctl set-default <target>` |
| Switch target immediately / 立即切换目标 | `sudo systemctl isolate <target>` |
| Identify file type (content) / 识别文件类型（内容） | `file <filepath>` |
| Identify file type (ls) / 识别文件类型（ls） | `ls -l <filepath>` |
| Third-party software directory / 第三方软件目录 | `/opt` |
| Block device files directory / 块设备文件目录 | `/dev` |
| Find hardware vendor info / 查找硬件供应商信息 | `sudo lshw -class network` |
| List mounted filesystems / 列出已挂载的文件系统 | `df -hP` |
| View hardware details / 查看硬件详情 | `sudo lshw -short` |

---

## Common systemd Target Reference
## 常用 systemd 目标参考

```bash
# Check current target / 检查当前目标
$ systemctl list-units --type=target --state=active

# Common targets and their equivalents / 常用目标及其等效运行级别
# poweroff.target  = runlevel 0  (shutdown / 关机)
# rescue.target    = runlevel 1  (single user / 单用户)
# multi-user.target = runlevel 3 (no GUI / 无图形界面)
# graphical.target = runlevel 5  (with GUI / 有图形界面)
# reboot.target    = runlevel 6  (reboot / 重启)

# Immediate shutdown / 立即关机
$ sudo systemctl poweroff
# or / 或
$ sudo poweroff

# Immediate reboot / 立即重启
$ sudo systemctl reboot
# or / 或
$ sudo reboot
```
