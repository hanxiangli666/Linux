# Linux Accounts
# Linux 账户体系

> In this section we will explore the foundational concepts of access control in Linux — how users and groups are structured, where account information is stored, and how to safely switch between users.
>
> 本节我们将探讨 Linux 访问控制的基础概念——用户和组的结构、账户信息的存储位置，以及如何安全地切换用户。

---

## User Accounts
## 用户账户

Every person or process that interacts with a Linux system does so through a **user account**. Each account has a unique numeric identifier called a **UID (User ID)**. The root account always has UID 0, making it the most privileged account on the system.

每个与 Linux 系统交互的人或进程都通过**用户账户**来进行。每个账户都有一个唯一的数字标识符，称为 **UID（用户 ID）**。root 账户的 UID 始终为 0，是系统上权限最高的账户。

User account information is stored in **`/etc/passwd`**. This file is readable by all users.

用户账户信息存储在 **`/etc/passwd`** 文件中，该文件所有用户都可读。

```bash
[~]$ cat /etc/passwd
```

Group information is stored in **`/etc/group`**.

组信息存储在 **`/etc/group`** 文件中。

```bash
[~]$ cat /etc/group
```

> **File Format of `/etc/passwd` `/etc/passwd` 文件格式：**
>
> ```
> michael:x:1001:1001:Michael Jones:/home/michael:/bin/bash
> USERNAME:PASSWORD:UID:GID:GECOS:HOMEDIR:SHELL
> ```
>
> - `x` in the password field means the actual password hash is stored in `/etc/shadow`
> - `x` 表示实际密码哈希存储在 `/etc/shadow` 中

---

### Checking User Details
### 查看用户详情

Use the `id` command to see a user's UID, GID, and group memberships:

使用 `id` 命令查看用户的 UID、GID 及所属组：

```bash
[~]$ id michael
uid=1001(michael) gid=1001(michael) groups=1001(michael),1003(developers)
```

To find more details (home directory, default shell) about a specific user, search `/etc/passwd`:

要查看用户的更多信息（家目录、默认 Shell），可以搜索 `/etc/passwd`：

```bash
[~]$ grep -i michael /etc/passwd
michael:x:1001:1001::/home/michael:/bin/bash
```

---

### Monitoring Logged-In Users
### 监控已登录用户

The **`who`** command shows all users currently logged into the system, along with their terminal and login time:

**`who`** 命令显示当前登录系统的所有用户，包括他们的终端和登录时间：

```bash
[~]$ who
bob      pts/2    Apr 28 06:48 (172.16.238.187)
michael  pts/3    Apr 28 07:15 (172.16.238.190)
```

The **`w`** command gives even more detail — including what each user is currently running:

**`w`** 命令提供更多详细信息，包括每个用户当前正在运行的进程：

```bash
[~]$ w
 08:00:01 up 3 days,  2:10,  2 users,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE JCPU PCPU WHAT
bob      pts/2    172.16.238.187   06:48    1:10  0.02s  0.02s -bash
michael  pts/3    172.16.238.190   07:15    5.00s 0.01s  0.01s top
```

The **`last`** command displays the login history of all users, including system reboots:

**`last`** 命令显示所有用户的登录历史记录，包括系统重启记录：

```bash
[~]$ last
michael  :1       :1           Tue May 12 20:00   still logged in
sarah    pts/1    172.16.238.X  Tue May 12 12:00 - 18:00  (06:00)
reboot   system boot  5.3.0-758-gen  Mon May 11 13:00 - 19:00  (06:00)
```

---

## Switching Users
## 切换用户

### The `su` Command
### `su` 命令

The `su` (substitute user) command allows you to switch to another user account. To switch to root:

`su`（substitute user）命令允许你切换到另一个用户账户。切换到 root 用户：

```bash
[~]$ su -
Password:

root@server:~#
```

> The `-` (dash) flag is important — it starts a **login shell**, loading the target user's full environment (PATH, variables, etc.). Without `-`, environment variables from the original user may leak over.
>
> `-`（破折号）标志很重要——它会启动一个**登录 Shell**，加载目标用户的完整环境（PATH、变量等）。不带 `-` 时，原用户的环境变量可能会泄漏过来。

To run a single command as another user without fully switching:

以其他用户身份运行单条命令而不完全切换：

```bash
[michael@ubuntu-server ~]$ su -c "whoami" root
Password:
root
```

> **Warning 警告:** Using `su` to run commands as root is generally **not recommended** because it requires sharing the root password. Use `sudo` instead.
>
> **警告：** 使用 `su` 以 root 身份运行命令通常**不推荐**，因为它需要共享 root 密码。请改用 `sudo`。

---

### The `sudo` Command — Preferred Method
### `sudo` 命令——推荐方式

`sudo` (superuser do) allows authorized users to run commands with elevated privileges **using their own password**, without needing the root password. This is the recommended approach for privilege escalation.

`sudo`（superuser do）允许授权用户使用**自己的密码**以提升的权限运行命令，无需 root 密码。这是提升权限的推荐方式。

```bash
[michael@ubuntu-server ~]$ sudo apt-get install nginx
[sudo] password for michael:
```

Users who are permitted to use `sudo` are listed in **`/etc/sudoers`**. This file should **always** be edited using `visudo` to prevent syntax errors:

被允许使用 `sudo` 的用户列在 **`/etc/sudoers`** 文件中。该文件**必须**使用 `visudo` 编辑，以防止语法错误：

```bash
[~]$ sudo visudo
```

A typical sudoers entry:
典型的 sudoers 条目：

```
# User privilege specification
root    ALL=(ALL:ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# Allow michael to run specific commands without a password
michael ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx
```

> **Best Practice 最佳实践:** Grant sudo access only for specific commands when possible, rather than giving full `ALL=(ALL) ALL` access. This limits the damage from compromised accounts.
>
> **最佳实践：** 尽可能只为特定命令授予 sudo 访问权限，而不是完全的 `ALL=(ALL) ALL` 访问权限。这可以限制账户被攻击时造成的损害。

---

## Restricting Root Login
## 限制 root 登录

To prevent anyone from logging in directly as root (a critical security hardening step), set the root shell to `nologin`:

为了防止任何人直接以 root 身份登录（这是一项关键的安全加固措施），将 root 的 shell 设置为 `nologin`：

```bash
[~]$ grep -i ^root /etc/passwd
root:x:0:0:root:/root:/usr/sbin/nologin
```

When set to `/usr/sbin/nologin`, any direct login attempt as root will be refused with a polite message. Users must instead use `sudo` to perform privileged operations. This is the default configuration on most modern Linux distributions (Ubuntu, Debian, etc.).

设置为 `/usr/sbin/nologin` 后，任何直接以 root 身份登录的尝试都会被拒绝，并显示一条提示信息。用户必须改用 `sudo` 执行特权操作。这是大多数现代 Linux 发行版（Ubuntu、Debian 等）的默认配置。

---

## User Types Summary
## 用户类型总结

| User Type 用户类型 | UID Range UID 范围 | Description 描述 |
| --- | --- | --- |
| **Root 超级用户** | 0 | Full system access 完全系统访问权限 |
| **System Users 系统用户** | 1 – 999 | Used by daemons and services 用于守护进程和服务 |
| **Regular Users 普通用户** | 1000+ | Human users with limited permissions 有限权限的普通人类用户 |

Understanding user types helps you apply the correct permissions and avoid over-privileged accounts that create security risks.

理解用户类型有助于你正确应用权限，避免产生安全风险的过度特权账户。
