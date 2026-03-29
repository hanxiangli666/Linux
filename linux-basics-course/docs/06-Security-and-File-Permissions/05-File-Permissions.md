# Linux File Permissions
# Linux 文件权限

> In this section we will learn about Linux file types, permission bits, how to read and modify permissions, and how to manage file ownership. Understanding file permissions is one of the most critical skills in Linux system administration.
>
> 本节我们将学习 Linux 文件类型、权限位、如何读取和修改权限，以及如何管理文件所有权。理解文件权限是 Linux 系统管理中最关键的技能之一。

---

## File Type Identifiers
## 文件类型标识符

Every file in Linux has a type, indicated by the **first character** of the permissions string in `ls -l` output.

Linux 中的每个文件都有一个类型，由 `ls -l` 输出中权限字符串的**第一个字符**表示。

```bash
[~]$ ls -la /home/bob/
drwxr-xr-x  5 bob  bob  4096 May 12 10:00 .
drwxr-xr-x 10 root root 4096 May 12 09:00 ..
-rw-r--r--  1 bob  bob   220 May 12 09:00 .bash_logout
lrwxrwxrwx  1 bob  bob     9 May 12 09:00 shortcut -> /tmp/data
```

| Character 字符 | File Type 文件类型 | Example 示例 |
| --- | --- | --- |
| `-` | Regular file 普通文件 | `/etc/passwd` |
| `d` | Directory 目录 | `/home/bob` |
| `l` | Symbolic link 符号链接 | `shortcut -> /tmp/data` |
| `c` | Character device 字符设备 | `/dev/tty` |
| `b` | Block device 块设备 | `/dev/sda` |
| `s` | Socket 套接字 | `/var/run/docker.sock` |
| `p` | Named pipe (FIFO) 命名管道 | `/tmp/mypipe` |

---

## Understanding the Permission String
## 理解权限字符串

The permissions of a file are displayed as a 10-character string. Let's break it down:

文件的权限显示为 10 字符字符串。让我们分解它：

```
-  rwx  r-x  r--
^  ^^^  ^^^  ^^^
|   |    |    |
|   |    |    └── Others (其他人): read only
|   |    └─────── Group (所属组): read + execute
|   └──────────── Owner/User (所有者): read + write + execute
└──────────────── File type (文件类型): - = regular file
```

### Permission Bits
### 权限位

Each entity (Owner, Group, Others) has three permission bits:

每个实体（所有者、组、其他人）都有三个权限位：

| Permission 权限 | Symbol 符号 | Numeric 数字 | On a File 对文件 | On a Directory 对目录 |
| --- | --- | --- | --- | --- |
| **Read** 读 | `r` | 4 | View file content 查看文件内容 | List directory contents 列出目录内容 |
| **Write** 写 | `w` | 2 | Modify file content 修改文件内容 | Create/delete files inside 在内部创建/删除文件 |
| **Execute** 执行 | `x` | 1 | Run the file as a program 将文件作为程序运行 | Enter (`cd`) the directory 进入（`cd`）目录 |
| **None** 无 | `-` | 0 | No permission 无权限 | No permission 无权限 |

> **Common Gotcha 常见陷阱:** For directories, the **execute** bit means "enter" not "run". Without execute on a directory, you can't `cd` into it or access files inside, even if you have read permission.
>
> **常见陷阱：** 对于目录，**执行**位意味着"进入"而不是"运行"。没有目录的执行权限，即使有读权限，也无法 `cd` 进入或访问其中的文件。

---

## Checking Permissions
## 检查权限

```bash
# List file permissions 列出文件权限
[~]$ ls -l test-file
-rw-r--r-- 1 bob developers 1024 May 12 10:00 test-file

# List directory permissions (use -d to show the dir itself, not its contents)
# 列出目录权限（使用 -d 显示目录本身而非其内容）
[~]$ ls -ld /home/bob/random_dir
drwxr-x--- 2 bob developers 4096 May 12 10:00 /home/bob/random_dir

# Show current user 显示当前用户
[~]$ whoami
bob
```

---

## Modifying Permissions with `chmod`
## 使用 `chmod` 修改权限

The **`chmod`** (change mode) command modifies file permissions. There are two syntax styles: **symbolic** and **octal (numeric)**.

**`chmod`**（change mode）命令修改文件权限。有两种语法风格：**符号式**和**八进制（数字）式**。

### Symbolic Mode
### 符号模式

The format is: `chmod [who][operator][permissions] file`

格式为：`chmod [谁][操作符][权限] 文件`

| Who 谁 | Meaning 含义 |
| --- | --- |
| `u` | User/Owner 用户/所有者 |
| `g` | Group 组 |
| `o` | Others 其他人 |
| `a` | All (u+g+o) 所有人 |

| Operator 操作符 | Meaning 含义 |
| --- | --- |
| `+` | Add permission 添加权限 |
| `-` | Remove permission 移除权限 |
| `=` | Set exact permission 设置精确权限 |

```bash
# Give the owner full access (read + write + execute)
# 给所有者完全访问权限（读+写+执行）
[~]$ chmod u+rwx test-file

# Add read access for everyone, remove execute from everyone
# 为所有人添加读权限，为所有人移除执行权限
[~]$ chmod a+r,a-x test-file

# Remove all access for others 移除其他人的所有权限
[~]$ chmod o-rwx test-file

# Complex: owner=rwx, group=r-x, others=--- (all in one command)
# 复杂：所有者=rwx，组=r-x，其他人=---（一条命令）
[~]$ chmod u+rwx,g+r-x,o-rwx test-file

# Set exact permissions using = 使用 = 设置精确权限
[~]$ chmod u=rw,g=r,o= test-file
```

### Octal (Numeric) Mode
### 八进制（数字）模式

Each permission combination maps to a number from 0–7:

每种权限组合映射到 0–7 的数字：

| Octal 八进制 | Binary 二进制 | Permissions 权限 | Meaning 含义 |
| --- | --- | --- | --- |
| `7` | 111 | `rwx` | Read + Write + Execute |
| `6` | 110 | `rw-` | Read + Write |
| `5` | 101 | `r-x` | Read + Execute |
| `4` | 100 | `r--` | Read only |
| `3` | 011 | `-wx` | Write + Execute |
| `2` | 010 | `-w-` | Write only |
| `1` | 001 | `--x` | Execute only |
| `0` | 000 | `---` | No permissions |

```bash
# 777 = rwxrwxrwx — full access for everyone (avoid on sensitive files!)
# 777 = 所有人完全访问（敏感文件避免使用！）
[~]$ chmod 777 test-file

# 755 = rwxr-xr-x — owner full, group/others read+execute (common for executables)
# 755 = 所有者完全，组/其他人读+执行（可执行文件常用）
[~]$ chmod 755 script.sh

# 644 = rw-r--r-- — owner read+write, others read-only (common for files)
# 644 = 所有者读+写，其他人只读（文件常用）
[~]$ chmod 644 config.txt

# 660 = rw-rw---- — owner and group read+write, others no access
# 660 = 所有者和组读+写，其他人无权限
[~]$ chmod 660 test-file

# 600 = rw------- — owner only, no access for anyone else (SSH private keys!)
# 600 = 仅所有者，其他人无权限（SSH 私钥！）
[~]$ chmod 600 ~/.ssh/id_rsa

# 750 = rwxr-x--- — owner full, group read+execute, others nothing
# 750 = 所有者完全，组读+执行，其他人无权限
[~]$ chmod 750 test-file
```

### Common Permission Patterns
### 常见权限模式

| Octal 八进制 | Use Case 使用场景 |
| --- | --- |
| `600` | Private SSH keys, sensitive config files 私有 SSH 密钥、敏感配置文件 |
| `644` | Regular text files, web content 普通文本文件、Web 内容 |
| `700` | Private user scripts 私有用户脚本 |
| `755` | System executables, public scripts 系统可执行文件、公共脚本 |
| `750` | Group-shared executables 组共享可执行文件 |
| `770` | Group-shared directories 组共享目录 |
| `775` | Collaborative project directories 协作项目目录 |

### Recursive Permission Change
### 递归权限更改

```bash
# Apply permissions to all files in a directory tree
# 对目录树中的所有文件应用权限
[~]$ chmod -R 755 /var/www/html
```

> **Warning 警告:** Be careful with recursive `chmod` — setting the same permissions on both files and directories can cause problems (e.g., making all files executable is usually wrong).
>
> **警告：** 对递归 `chmod` 要谨慎——对文件和目录设置相同权限可能会导致问题（例如，使所有文件可执行通常是错误的）。

---

## Special Permission Bits
## 特殊权限位

Beyond the basic rwx bits, Linux has three special permission bits:

除了基本的 rwx 位，Linux 还有三个特殊权限位：

### SUID (Set User ID) — Octal 4xxx
### SUID（设置用户 ID）——八进制 4xxx

When set on an **executable**, the program runs with the **file owner's** privileges, not the calling user's.

当设置在**可执行文件**上时，程序以**文件所有者**的权限运行，而不是调用用户的权限。

```bash
# Classic example: passwd command (needs to write /etc/shadow as root)
# 经典示例：passwd 命令（需要以 root 身份写入 /etc/shadow）
[~]$ ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root 68208 Nov 29 2022 /usr/bin/passwd
#    ^--- 's' indicates SUID is set
```

### SGID (Set Group ID) — Octal 2xxx
### SGID（设置组 ID）——八进制 2xxx

On a **directory**: new files created inside inherit the directory's **group** (useful for shared directories).
On an **executable**: the program runs with the **file's group** privileges.

在**目录**上：在内部创建的新文件继承目录的**组**（对共享目录有用）。
在**可执行文件**上：程序以**文件所属组**的权限运行。

```bash
# Set SGID on a shared project directory
# 在共享项目目录上设置 SGID
[~]$ chmod g+s /home/mercury-team/shared
[~]$ ls -ld /home/mercury-team/shared
drwxrws--- 2 root mercury-team 4096 May 12 10:00 /home/mercury-team/shared
#      ^--- 's' indicates SGID is set
```

### Sticky Bit — Octal 1xxx
### 粘滞位——八进制 1xxx

On a **directory**: users can only delete or rename **their own files**, even if they have write permission on the directory. Essential for shared directories like `/tmp`.

在**目录**上：用户只能删除或重命名**自己的文件**，即使他们对目录有写权限。对于 `/tmp` 等共享目录至关重要。

```bash
[~]$ ls -ld /tmp
drwxrwxrwt 10 root root 4096 May 12 10:00 /tmp
#         ^--- 't' indicates sticky bit is set

# Set sticky bit on a shared directory
# 在共享目录上设置粘滞位
[~]$ chmod +t /home/shared-uploads
```

---

## Changing Ownership with `chown` and `chgrp`
## 使用 `chown` 和 `chgrp` 更改所有权

### `chown` — Change Owner and/or Group
### `chown` — 更改所有者和/或组

```bash
# Change both owner and group 同时更改所有者和组
[~]$ chown bob:developer test-file

# Change only the owner (group unchanged) 只更改所有者（组不变）
[~]$ chown bob test-file

# Change only the group (colon with no owner)
# 只更改组（冒号前不写所有者）
[~]$ chown :developer test-file

# Recursively change ownership of a directory tree
# 递归更改目录树的所有权
[~]$ chown -R bob:developer /home/mercury-project/
```

### `chgrp` — Change Group Only
### `chgrp` — 只更改组

```bash
# Change the group for test-file to 'android'
# 将 test-file 的组更改为 android
[~]$ chgrp android test-file

# Recursively change group for all files
# 递归更改所有文件的组
[~]$ chgrp -R developers /home/mercury-project/
```

---

## umask — Default Permission Mask
## umask — 默认权限掩码

The **umask** determines the default permissions for newly created files and directories.

**umask** 决定新创建的文件和目录的默认权限。

```bash
[~]$ umask
0022
```

- Default file permissions = `666` (rw-rw-rw-) minus umask `022` = `644` (rw-r--r--)
- Default directory permissions = `777` (rwxrwxrwx) minus umask `022` = `755` (rwxr-xr-x)

- 文件默认权限 = `666` (rw-rw-rw-) 减去 umask `022` = `644` (rw-r--r--)
- 目录默认权限 = `777` (rwxrwxrwx) 减去 umask `022` = `755` (rwxr-xr-x)

```bash
# Set a more restrictive umask (new files: 600, new dirs: 700)
# 设置更严格的 umask（新文件：600，新目录：700）
[~]$ umask 077
```

---

## Hands-On Practice
## 动手实践

- File Permissions Labs: [Take me to Labs](https://kodekloud.com/courses/873064/lectures/17074516)
- 文件权限实验室：[前往实验室](https://kodekloud.com/courses/873064/lectures/17074516)
