# Access Control Files
# 访问控制文件

> Linux uses several key files under `/etc` to control user authentication and authorization. Understanding these files is essential for system administration and security auditing.
>
> Linux 使用 `/etc` 目录下的几个关键文件来控制用户认证和授权。理解这些文件对于系统管理和安全审计至关重要。

---

## Overview
## 概述

The primary access control files are stored under **`/etc`**. These files can be **read by any user** but can only be **modified by the root user**. They form the backbone of Linux's local authentication system.

主要的访问控制文件存储在 **`/etc`** 目录下。这些文件**所有用户都可读**，但只有 **root 用户才能修改**。它们构成了 Linux 本地认证系统的骨干。

| File 文件 | Purpose 用途 |
| --- | --- |
| `/etc/passwd` | User account information 用户账户信息 |
| `/etc/shadow` | Encrypted passwords + aging policy 加密密码 + 老化策略 |
| `/etc/group` | Group definitions 组定义 |
| `/etc/gshadow` | Secure group passwords 安全的组密码 |
| `/etc/sudoers` | Sudo privilege definitions sudo 权限定义 |

---

## /etc/passwd — User Account Database
## /etc/passwd — 用户账户数据库

This file contains basic information for every user account on the system. Each line represents one user, with fields separated by colons (`:`).

此文件包含系统上每个用户账户的基本信息。每行代表一个用户，字段之间用冒号（`:`）分隔。

```bash
[~]$ grep -i ^bob /etc/passwd
bob:x:1002:1002:Bob Smith:/home/bob:/bin/bash
```

### Field Breakdown
### 字段解析

```
bob       :  x  :  1002  :  1002  :  Bob Smith  :  /home/bob  :  /bin/bash
USERNAME  :  PWD :  UID   :  GID   :  GECOS      :  HOMEDIR    :  SHELL
```

| Field 字段 | Description 描述 |
| --- | --- |
| **USERNAME** | Login name (must be unique) 登录名（必须唯一） |
| **PASSWORD** | `x` = password stored in `/etc/shadow`; `*` = no login allowed `x` = 密码存储在 `/etc/shadow`；`*` = 不允许登录 |
| **UID** | User ID; root=0, system users=1-999, regular users=1000+ 用户 ID；root=0，系统用户=1-999，普通用户=1000+ |
| **GID** | Primary Group ID 主组 ID |
| **GECOS** | Comment field — usually full name or description 注释字段——通常是全名或描述 |
| **HOMEDIR** | Absolute path to the user's home directory 用户家目录的绝对路径 |
| **SHELL** | Default login shell 默认登录 Shell |

> **Security Note 安全说明:** In older UNIX systems, the actual password was stored in this field. Modern Linux moves it to `/etc/shadow` because `/etc/passwd` is world-readable — storing passwords here would be a major security risk.
>
> **安全说明：** 在旧版 UNIX 系统中，实际密码存储在此字段中。现代 Linux 将其移至 `/etc/shadow`，因为 `/etc/passwd` 对所有人可读——在此处存储密码将是重大安全风险。

---

## /etc/shadow — Secure Password Storage
## /etc/shadow — 安全密码存储

The shadow file stores password hashes and password aging policies. It is readable **only by root** (or programs with `CAP_DAC_READ_SEARCH` capability).

shadow 文件存储密码哈希和密码老化策略。只有 **root**（或具有 `CAP_DAC_READ_SEARCH` 能力的程序）才能读取。

```bash
[~]$ sudo grep -i ^bob /etc/shadow
bob:$6$0h0utOtO$5JcuRxR7y72LLQk4Kdog7u09LsNFS0yZP...:18188:0:99999:7:::
```

### Field Breakdown
### 字段解析

```
USERNAME : HASHED_PASSWORD : LASTCHANGE : MINAGE : MAXAGE : WARN : INACTIVE : EXPDATE : RESERVED
```

| Field 字段 | Description 描述 | Example 示例 |
| --- | --- | --- |
| **USERNAME** | Login name 登录名 | `bob` |
| **HASHED_PASSWORD** | Password hash (algorithm prefix indicates hash type) 密码哈希（算法前缀指示哈希类型） | `$6$...` = SHA-512 |
| **LASTCHANGE** | Days since epoch when password was last changed 密码最后更改距 epoch 的天数 | `18188` |
| **MINAGE** | Minimum days before password can be changed 密码可更改的最少天数 | `0` = no minimum |
| **MAXAGE** | Maximum days before password must be changed 密码必须更改的最大天数 | `99999` = no expiry |
| **WARN** | Days before expiry to warn the user 到期前警告用户的天数 | `7` |
| **INACTIVE** | Days after expiry before account is disabled 过期后禁用账户的天数 | (empty = never) |
| **EXPDATE** | Absolute account expiration date (days since epoch) 账户绝对过期日期（距 epoch 的天数） | (empty = never) |

### Password Hash Algorithms
### 密码哈希算法

The prefix of the hash string tells you which algorithm was used:

哈希字符串的前缀告诉你使用了哪种算法：

| Prefix 前缀 | Algorithm 算法 | Security 安全性 |
| --- | --- | --- |
| `$1$` | MD5 | Weak — avoid 弱，避免使用 |
| `$5$` | SHA-256 | Acceptable 可接受 |
| `$6$` | SHA-512 | Recommended 推荐 |
| `$y$` | yescrypt | Modern standard 现代标准 |

### Special Password Field Values
### 密码字段特殊值

| Value 值 | Meaning 含义 |
| --- | --- |
| `!` or `!!` | Account is locked 账户被锁定 |
| `*` | No password login possible 无法密码登录（系统账户） |
| (empty) | No password required — **dangerous!** 不需要密码——**危险！** |

---

## /etc/group — Group Database
## /etc/group — 组数据库

This file defines all groups on the system and their memberships.

此文件定义系统上的所有组及其成员关系。

```bash
[~]$ grep -i ^developers /etc/group
developers:x:1003:bob,alice,michael
```

### Field Breakdown
### 字段解析

```
developers : x  : 1003 : bob,alice,michael
NAME       : PWD : GID  : MEMBERS
```

| Field 字段 | Description 描述 |
| --- | --- |
| **NAME** | Group name (must be unique) 组名（必须唯一） |
| **PASSWORD** | `x` = password in `/etc/gshadow`; rarely used `x` = 密码在 `/etc/gshadow` 中；很少使用 |
| **GID** | Group ID (unique number) 组 ID（唯一编号） |
| **MEMBERS** | Comma-separated list of supplementary group members 附加组成员的逗号分隔列表 |

> **Primary vs. Supplementary Groups 主组与附加组：**
> - A user's **primary group** is defined in `/etc/passwd` (the GID field) — files created by that user belong to this group by default.
> - **Supplementary groups** are listed in `/etc/group` — they grant additional access rights.
>
> - 用户的**主组**在 `/etc/passwd` 中定义（GID 字段）——该用户创建的文件默认属于此组。
> - **附加组**在 `/etc/group` 中列出——它们授予额外的访问权限。

To check which groups a user belongs to:

检查用户属于哪些组：

```bash
[~]$ groups bob
bob : bob developers sudo

[~]$ id bob
uid=1002(bob) gid=1002(bob) groups=1002(bob),27(sudo),1003(developers)
```

---

## /etc/gshadow — Secure Group Passwords
## /etc/gshadow — 安全组密码

Similar to `/etc/shadow` but for groups. Contains group passwords and administrator information. Group passwords are rarely used in modern setups.

类似于 `/etc/shadow`，但用于组。包含组密码和管理员信息。现代设置中很少使用组密码。

```bash
[~]$ sudo grep -i ^developers /etc/gshadow
developers:!::bob,alice
```

---

## Practical: Auditing Access Control Files
## 实践：审计访问控制文件

Here are useful commands for security auditing:

以下是用于安全审计的实用命令：

```bash
# Find accounts with no password (empty password field)
# 查找没有密码的账户（空密码字段）
[~]$ sudo awk -F: '($2 == "") {print $1}' /etc/shadow

# Find accounts with UID 0 (potential backdoor roots)
# 查找 UID 为 0 的账户（潜在的 root 后门）
[~]$ awk -F: '($3 == "0") {print $1}' /etc/passwd

# Find locked accounts 查找已锁定的账户
[~]$ sudo grep -E '^[^:]+:!' /etc/shadow | cut -d: -f1

# List all users with login shells (not system accounts)
# 列出所有有登录 Shell 的用户（非系统账户）
[~]$ grep -v '/nologin\|/false' /etc/passwd | cut -d: -f1,7

# Check sudo group members 检查 sudo 组成员
[~]$ grep sudo /etc/group
```

---

## File Permissions on Control Files
## 控制文件的文件权限

These files have carefully set permissions. You can verify them with:

这些文件设置了精心配置的权限。你可以通过以下命令验证：

```bash
[~]$ ls -la /etc/passwd /etc/shadow /etc/group /etc/gshadow
-rw-r--r-- 1 root root   2847 May 12 10:00 /etc/passwd
-rw-r----- 1 root shadow 1673 May 12 10:00 /etc/shadow
-rw-r--r-- 1 root root    892 May 12 10:00 /etc/group
-rw-r----- 1 root shadow  712 May 12 10:00 /etc/gshadow
```

> If these permissions are wrong (e.g., `/etc/shadow` is world-readable), this is a **critical security vulnerability** that must be fixed immediately.
>
> 如果这些权限设置错误（例如 `/etc/shadow` 对所有人可读），这是一个**严重的安全漏洞**，必须立即修复。

```bash
# Fix shadow file permissions if they are wrong
# 如果权限错误，修复 shadow 文件权限
[~]$ sudo chmod 640 /etc/shadow
[~]$ sudo chown root:shadow /etc/shadow
```

---

## Hands-On Practice
## 动手实践

- Managing User Accounts: [Take me to Labs](https://kodekloud.com/courses/the-linux-basics-course/lectures/17074503)
- 用户账户管理：[前往实验室](https://kodekloud.com/courses/the-linux-basics-course/lectures/17074503)
