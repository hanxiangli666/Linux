# SSH and SCP
# SSH 与 SCP

> In this section we will learn about SSH (Secure Shell) for remote login and SCP (Secure Copy Protocol) for transferring files. These are the two most commonly used tools for remote server management in Linux.
>
> 本节我们将学习用于远程登录的 SSH（安全外壳协议）和用于文件传输的 SCP（安全复制协议）。这是 Linux 远程服务器管理中最常用的两个工具。

---

## What is SSH?
## 什么是 SSH？

**SSH (Secure Shell)** is a cryptographic network protocol that provides a secure channel over an unsecured network. It replaces older, insecure protocols like Telnet and rsh where data (including passwords) was transmitted in **plain text**.

**SSH（安全外壳协议）**是一种加密网络协议，可在不安全的网络上提供安全通道。它替代了 Telnet 和 rsh 等较旧的不安全协议——在这些协议中，数据（包括密码）以**明文**传输。

SSH provides:
SSH 提供：

- **Encrypted communication** — all data in transit is encrypted 加密通信——所有传输中的数据都已加密
- **Host authentication** — verifies you're connecting to the right server 主机认证——验证你连接的是正确的服务器
- **User authentication** — supports passwords, public keys, and certificates 用户认证——支持密码、公钥和证书
- **Port forwarding / tunneling** — securely tunnel other protocols 端口转发/隧道——安全地隧道化其他协议

SSH typically runs on **port 22**.

SSH 通常在**端口 22**上运行。

---

## Basic SSH Usage
## SSH 基本用法

### Connecting to a Remote Server
### 连接到远程服务器

```bash
# Connect using hostname or IP (uses current username)
# 使用主机名或 IP 连接（使用当前用户名）
ssh devapp01
ssh 192.168.1.100

# Connect with a specific username
# 使用指定用户名连接
ssh bob@devapp01
ssh bob@192.168.1.100

# Using the -l flag (equivalent to user@host)
# 使用 -l 标志（等同于 user@host）
ssh -l bob devapp01

# Connect on a non-standard port (e.g., 2222)
# 连接到非标准端口（例如 2222）
ssh -p 2222 bob@devapp01
```

### First-Time Connection — Host Key Verification
### 首次连接——主机密钥验证

When connecting to a new server for the first time, SSH displays the server's fingerprint and asks you to confirm:

首次连接到新服务器时，SSH 会显示服务器的指纹并要求你确认：

```
The authenticity of host 'devapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:abc123xyz...
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'devapp01' to the list of known hosts.
```

> **Security Note 安全说明:** Always verify the fingerprint out-of-band (e.g., via the server console) before accepting — this protects against **Man-in-the-Middle (MITM)** attacks.
>
> **安全说明：** 在接受之前，始终通过带外方式（例如通过服务器控制台）验证指纹——这可以防止**中间人（MITM）攻击**。

Known hosts are stored in `~/.ssh/known_hosts`.
已知主机存储在 `~/.ssh/known_hosts` 中。

---

## Password-Less Authentication with Key Pairs
## 使用密钥对进行无密码认证

Password-based SSH login is convenient but less secure (vulnerable to brute-force attacks). **Public key authentication** is strongly preferred for production systems.

基于密码的 SSH 登录方便但安全性较低（容易受到暴力破解攻击）。**公钥认证**在生产系统中是强烈推荐的方式。

### How Key-Pair Authentication Works
### 密钥对认证的工作原理

1. You generate a **key pair**: a private key (secret, stays on your machine) and a public key (shared with servers).
2. The public key is copied to the server's `~/.ssh/authorized_keys` file.
3. When you SSH in, the server challenges your client, your client proves it has the matching private key — **no password ever crosses the network**.

1. 你生成一个**密钥对**：私钥（保密，保留在你的机器上）和公钥（与服务器共享）。
2. 公钥被复制到服务器的 `~/.ssh/authorized_keys` 文件中。
3. 当你 SSH 登录时，服务器向你的客户端发起质询，你的客户端证明它拥有匹配的私钥——**网络上永远不会传递密码**。

### Step 1: Generate a Key Pair
### 第一步：生成密钥对

```bash
# Generate an RSA key pair (4096 bits recommended)
# 生成 RSA 密钥对（推荐 4096 位）
[bob@caleston-lp10 ~]$ ssh-keygen -t rsa -b 4096 -C "bob@caleston-lp10"

# Or generate an Ed25519 key (modern, faster, equally secure)
# 或者生成 Ed25519 密钥（现代、更快、同等安全）
[bob@caleston-lp10 ~]$ ssh-keygen -t ed25519 -C "bob@caleston-lp10"

Generating public/private rsa key pair.
Enter file in which to save the key (/home/bob/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):        # Strongly recommended!
Enter same passphrase again:
Your identification has been saved in /home/bob/.ssh/id_rsa
Your public key has been saved in /home/bob/.ssh/id_rsa.pub
```

> **Passphrase 口令:** Adding a passphrase to your private key provides an extra layer of security. Even if someone steals your key file, they can't use it without the passphrase. Use `ssh-agent` to avoid re-entering it constantly.
>
> **口令：** 为私钥添加口令提供了额外的安全层。即使有人窃取了你的密钥文件，没有口令也无法使用它。使用 `ssh-agent` 可以避免不断重新输入口令。

Key locations:
密钥位置：

```
Private Key (私钥): /home/bob/.ssh/id_rsa       (permissions must be 600!)
Public Key  (公钥): /home/bob/.ssh/id_rsa.pub
```

### Step 2: Copy Public Key to the Remote Server
### 第二步：将公钥复制到远程服务器

```bash
# Recommended: use ssh-copy-id (handles authorized_keys automatically)
# 推荐：使用 ssh-copy-id（自动处理 authorized_keys）
[bob@caleston-lp10 ~]$ ssh-copy-id bob@devapp01
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/bob/.ssh/id_rsa.pub"
bob@devapp01's password:
Number of key(s) added: 1

# Manual alternative (if ssh-copy-id is not available)
# 手动替代方案（如果 ssh-copy-id 不可用）
[bob@caleston-lp10 ~]$ cat ~/.ssh/id_rsa.pub | ssh bob@devapp01 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

### Step 3: Login Without Password
### 第三步：无密码登录

```bash
[bob@caleston-lp10 ~]$ ssh devapp01
# No password prompt! Authenticated via private key.
# 没有密码提示！通过私钥认证。

Welcome to Ubuntu 20.04.3 LTS
bob@devapp01:~$
```

### Verify: View Authorized Keys on the Server
### 验证：查看服务器上的授权密钥

```bash
[bob@devapp01 ~]$ cat ~/.ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAC... bob@caleston-lp10
```

---

## SSH Configuration
## SSH 配置

### Client Configuration (`~/.ssh/config`)
### 客户端配置（`~/.ssh/config`）

You can create shortcuts for frequently accessed servers:

你可以为常用服务器创建快捷方式：

```bash
# ~/.ssh/config
Host devapp01
    HostName 172.16.238.10
    User bob
    Port 22
    IdentityFile ~/.ssh/id_rsa

Host prod-db
    HostName 10.0.0.50
    User dbadmin
    Port 2222
    IdentityFile ~/.ssh/id_ed25519
```

Now you can simply type:
现在你可以简单地输入：

```bash
[~]$ ssh devapp01       # Instead of: ssh -i ~/.ssh/id_rsa bob@172.16.238.10
[~]$ ssh prod-db
```

### Server Configuration (`/etc/ssh/sshd_config`)
### 服务器配置（`/etc/ssh/sshd_config`）

Key security settings on the server side:

服务器端的关键安全设置：

```bash
# Disable root login via SSH (critical security hardening!)
# 禁用通过 SSH 的 root 登录（关键安全加固！）
PermitRootLogin no

# Disable password authentication (use keys only)
# 禁用密码认证（仅使用密钥）
PasswordAuthentication no

# Change default port to reduce automated scanning
# 更改默认端口以减少自动扫描
Port 2222

# Allow only specific users
# 只允许特定用户
AllowUsers bob alice
```

After modifying `/etc/ssh/sshd_config`, restart the SSH service:
修改 `/etc/ssh/sshd_config` 后，重启 SSH 服务：

```bash
[~]$ sudo systemctl restart sshd
```

---

## SCP — Secure Copy Protocol
## SCP — 安全复制协议

**SCP** uses SSH to transfer files securely between hosts. Think of it as `cp` but over the network.

**SCP** 使用 SSH 在主机之间安全地传输文件。可以把它看作是通过网络的 `cp`。

### Copying Files
### 复制文件

```bash
# Copy a local file to a remote server
# 将本地文件复制到远程服务器
[bob@caleston-lp10 ~]$ scp /home/bob/caleston-code.tar.gz devapp01:/home/bob/
[bob@caleston-lp10 ~]$ scp report.pdf bob@192.168.1.100:/tmp/

# Copy a remote file to local machine
# 将远程文件复制到本地机器
[bob@caleston-lp10 ~]$ scp devapp01:/home/bob/server.log /tmp/

# Specify a non-standard SSH port with -P (capital P!)
# 使用 -P 指定非标准 SSH 端口（大写 P！）
[bob@caleston-lp10 ~]$ scp -P 2222 report.pdf bob@devapp01:/home/bob/
```

### Copying Directories
### 复制目录

```bash
# Copy an entire directory recursively (-r flag)
# 递归复制整个目录（-r 标志）
[bob@caleston-lp10 ~]$ scp -r /home/bob/media/ devapp01:/home/bob/

# Preserve file timestamps and permissions (-p flag)
# 保留文件时间戳和权限（-p 标志）
[bob@caleston-lp10 ~]$ scp -pr /home/bob/media/ devapp01:/home/bob/
```

---

## rsync — A More Powerful Alternative to SCP
## rsync — 比 SCP 更强大的替代方案

For large transfers or synchronization tasks, **`rsync`** is preferred over SCP. It only transfers changed parts of files (delta transfer), making it much faster for incremental backups.

对于大型传输或同步任务，**`rsync`** 比 SCP 更受欢迎。它只传输文件的变更部分（增量传输），使增量备份速度更快。

```bash
# Sync a directory to a remote server (with progress and archive mode)
# 将目录同步到远程服务器（带进度和归档模式）
[~]$ rsync -avzP /home/bob/project/ bob@devapp01:/home/bob/project/

# Options:
# -a: archive mode (preserve permissions, timestamps, symlinks, etc.)
# -v: verbose
# -z: compress data during transfer
# -P: show progress + keep partial files
```

---

## Summary of Key Commands
## 关键命令总结

| Command 命令 | Description 描述 |
| --- | --- |
| `ssh user@host` | Connect to remote host 连接到远程主机 |
| `ssh-keygen -t ed25519` | Generate SSH key pair 生成 SSH 密钥对 |
| `ssh-copy-id user@host` | Copy public key to server 将公钥复制到服务器 |
| `scp file user@host:/path` | Copy file to remote 将文件复制到远程 |
| `scp -r dir user@host:/path` | Copy directory to remote 将目录复制到远程 |
| `rsync -avzP src user@host:dst` | Sync with delta transfer 使用增量传输同步 |
