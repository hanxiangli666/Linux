# User Management
# 用户管理

> In this section we will learn how to create, modify, and delete user accounts and groups in Linux. Proper user management is the foundation of a secure, well-organized system.
>
> 本节我们将学习如何在 Linux 中创建、修改和删除用户账户及用户组。良好的用户管理是构建安全、有序系统的基础。

---

## Adding Users
## 添加用户

### Basic User Creation
### 基本用户创建

Use the **`useradd`** command to create a new local user. The simplest form:

使用 **`useradd`** 命令创建新的本地用户。最简单的用法：

```bash
[~]$ useradd bob
```

After creation, verify the account in `/etc/passwd`:

创建后，在 `/etc/passwd` 中验证账户：

```bash
[~]$ grep -i bob /etc/passwd
bob:x:1002:1002::/home/bob:/bin/sh
```

> **Note 注意:** On some distributions (e.g., Ubuntu), `adduser` is a friendlier interactive wrapper around `useradd` that automatically creates home directories, sets passwords, and prompts for user information.
>
> **注意：** 在某些发行版（如 Ubuntu）上，`adduser` 是 `useradd` 的友好交互式封装，会自动创建家目录、设置密码并提示用户信息。

### Advanced User Creation with Options
### 带选项的高级用户创建

The `useradd` command supports many options to configure the account at creation time:

`useradd` 命令支持许多选项，可在创建时配置账户：

```bash
[~]$ useradd -u 1009 -g 1009 -d /home/robert -s /bin/bash -c "Mercury Project member" bob
```

| Option 选项 | Description 描述 | Example 示例 |
| --- | --- | --- |
| `-u` | Specify UID 指定 UID | `-u 1009` |
| `-g` | Specify primary GID 指定主组 GID | `-g 1009` |
| `-G` | Add to supplementary groups 添加到附加组 | `-G developers,ops` |
| `-d` | Set home directory 设置家目录 | `-d /home/robert` |
| `-s` | Set login shell 设置登录 Shell | `-s /bin/bash` |
| `-c` | Add a comment/description 添加注释/描述 | `-c "Mercury Project member"` |
| `-m` | Create home directory if it doesn't exist 创建家目录（如不存在） | `-m` |
| `-e` | Set account expiration date 设置账户过期日期 | `-e 2025-12-31` |
| `-L` | Lock the account after creation 创建后锁定账户 | `-L` |

---

## Checking the Current User
## 检查当前用户

Use **`whoami`** to display the username of the currently logged-in user:

使用 **`whoami`** 显示当前登录用户的用户名：

```bash
[~]$ whoami
bob
```

To get a full picture including UID, GID, and groups, use `id`:

要获取包括 UID、GID 和组的完整信息，使用 `id`：

```bash
[~]$ id
uid=1002(bob) gid=1002(bob) groups=1002(bob),1003(developers)
```

---

## Managing Passwords
## 管理密码

All user passwords are stored (as secure hashes) in **`/etc/shadow`**, which is readable only by root:

所有用户密码以安全哈希形式存储在 **`/etc/shadow`** 中，只有 root 才能读取：

```bash
[~]$ grep -i bob /etc/shadow
bob:!:18341:0:99999:7:::
```

> The `!` in the password field means the account is **locked** (no password set yet). A `*` also means the account cannot be used for password-based login.
>
> 密码字段中的 `!` 表示账户被**锁定**（尚未设置密码）。`*` 同样表示账户无法用于基于密码的登录。

To set or change a user's password:

设置或更改用户密码：

```bash
# Change password for the current user 更改当前用户密码
[~]$ passwd

# Change password for a specific user (requires root) 更改指定用户密码（需要 root 权限）
[~]$ passwd bob
Changing password for user bob.
New UNIX password:
Retype new UNIX password:
passwd: all authentication tokens updated successfully.
```

### Password Aging Policies
### 密码老化策略

Use **`chage`** to configure password expiration policies — an important security requirement in many organizations:

使用 **`chage`** 配置密码过期策略——这是许多组织的重要安全要求：

```bash
# View password aging info for bob 查看 bob 的密码老化信息
[~]$ chage -l bob

# Force bob to change password on next login 强制 bob 下次登录时更改密码
[~]$ chage -d 0 bob

# Set password to expire after 90 days 设置密码90天后过期
[~]$ chage -M 90 bob

# Set warning 14 days before expiry 设置过期前14天警告
[~]$ chage -W 14 bob
```

---

## Modifying Users
## 修改用户

Use **`usermod`** to modify an existing user account:

使用 **`usermod`** 修改现有用户账户：

```bash
# Change bob's login shell to bash 将 bob 的登录 Shell 改为 bash
[~]$ usermod -s /bin/bash bob

# Add bob to the 'developers' group (without removing from other groups)
# 将 bob 添加到 developers 组（不从其他组移除）
[~]$ usermod -aG developers bob

# Lock bob's account (disable login) 锁定 bob 的账户（禁止登录）
[~]$ usermod -L bob

# Unlock bob's account 解锁 bob 的账户
[~]$ usermod -U bob

# Change bob's home directory and move files
# 更改 bob 的家目录并移动文件
[~]$ usermod -d /home/robert -m bob
```

> **Important 重要:** Always use `-aG` (append + group) when adding a user to supplementary groups. Using `-G` alone **overwrites** all existing supplementary group memberships!
>
> **重要：** 将用户添加到附加组时，始终使用 `-aG`（追加+组）。单独使用 `-G` 会**覆盖**所有现有的附加组成员资格！

---

## Deleting Users
## 删除用户

Use **`userdel`** to remove a user:

使用 **`userdel`** 删除用户：

```bash
# Delete user but keep home directory 删除用户但保留家目录
[~]$ userdel bob

# Delete user AND their home directory 删除用户及其家目录
[~]$ userdel -r bob
```

> **Warning 警告:** `userdel -r` is irreversible — it permanently removes the user's home directory and mail spool. Always back up important data first.
>
> **警告：** `userdel -r` 操作不可逆——它会永久删除用户的家目录和邮件队列。请务必先备份重要数据。

---

## Group Management
## 组管理

Groups are used to organize users and manage shared access to resources. Every user belongs to a **primary group** and can belong to multiple **supplementary groups**.

组用于组织用户并管理对资源的共享访问。每个用户都属于一个**主组**，并可以属于多个**附加组**。

### Creating Groups
### 创建组

```bash
# Create a group with a specific GID 使用指定 GID 创建组
[~]$ groupadd -g 1011 developer

# Create group with auto-assigned GID 使用自动分配的 GID 创建组
[~]$ groupadd testers
```

### Modifying Groups
### 修改组

```bash
# Rename a group 重命名组
[~]$ groupmod -n devteam developer

# Change a group's GID 更改组的 GID
[~]$ groupmod -g 2011 developer
```

### Deleting Groups
### 删除组

```bash
[~]$ groupdel developer
```

> **Note 注意:** You cannot delete a group that is the **primary group** of any existing user. Remove or reassign those users first.
>
> **注意：** 不能删除作为任何现有用户**主组**的组。请先移除或重新分配这些用户。

---

## Practical Example: Setting Up a Project Team
## 实践示例：设置项目团队

Here's a complete example of creating a development team for "Project Mercury":

以下是为"Mercury 项目"创建开发团队的完整示例：

```bash
# 1. Create the project group 创建项目组
groupadd -g 2001 mercury-team

# 2. Create team members 创建团队成员
useradd -u 2010 -g 2001 -G sudo -d /home/alice -s /bin/bash -m -c "Mercury Lead Dev" alice
useradd -u 2011 -g 2001 -d /home/bob -s /bin/bash -m -c "Mercury Dev" bob

# 3. Set passwords 设置密码
passwd alice
passwd bob

# 4. Force password change on first login 强制首次登录时更改密码
chage -d 0 alice
chage -d 0 bob

# 5. Verify 验证
grep mercury-team /etc/group
id alice
id bob
```

---

## Summary of Key Commands
## 关键命令总结

| Command 命令 | Purpose 用途 |
| --- | --- |
| `useradd [options] username` | Create a new user 创建新用户 |
| `usermod [options] username` | Modify an existing user 修改现有用户 |
| `userdel [-r] username` | Delete a user 删除用户 |
| `passwd [username]` | Set/change a password 设置/更改密码 |
| `chage [options] username` | Manage password aging 管理密码老化策略 |
| `groupadd [-g GID] groupname` | Create a new group 创建新组 |
| `groupmod [options] groupname` | Modify a group 修改组 |
| `groupdel groupname` | Delete a group 删除组 |
| `id [username]` | Show user/group IDs 显示用户/组 ID |
| `whoami` | Show current username 显示当前用户名 |
