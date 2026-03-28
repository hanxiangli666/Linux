# Lab - RPM and YUM
# 实验 - RPM 与 YUM

- Access Hands-On Labs here [Hands-On Labs](https://kodekloud.com/topic/lab-yum-and-rpm/)

This lab covers practical exercises using RPM and YUM on a CentOS/RHEL system. Each exercise includes the command, detailed explanation, and additional context.

本实验涵盖在 CentOS/RHEL 系统上使用 RPM 和 YUM 的实践练习。每道题都包含命令、详细解释以及扩展内容。

---

### Exercise 1 — Which Package Managers to Use on CentOS
### 练习 1 — CentOS 上使用哪些包管理器

**Question / 问题**: Which package managers would you use on a CentOS machine?

**Answer / 答案**: CentOS uses **RPM** (low-level) and **YUM** (high-level).

```
RPM  → Low-level, works with .rpm files directly, NO automatic dependency resolution
        底层工具，直接处理 .rpm 文件，不自动解析依赖

YUM  → High-level, uses repositories, AUTOMATIC dependency resolution
        高层工具，使用软件仓库，自动解析依赖
```

> **When to use which / 何时使用哪个:**
> - Use **YUM** for day-to-day package management — it handles everything automatically / 日常包管理使用 **YUM**——它自动处理一切
> - Use **RPM** when you need to inspect or verify installed packages, or install a specific `.rpm` file in offline environments / 当需要检查或验证已安装的包，或在离线环境中安装特定 `.rpm` 文件时使用 **RPM**

---

### Exercise 2 — Find the Exact Package Name for `wget`
### 练习 2 — 查找 `wget` 的确切包名

Use an `rpm` command to find out the exact package name for `wget` installed in the system.

使用 `rpm` 命令查找系统中已安装的 `wget` 的确切包名。

```bash
$ rpm -qa | grep wget
wget-1.14-18.el7_6.1.x86_64
```

**Explanation / 说明:**
- `rpm -qa` — **q**uery **a**ll installed packages / 查询**所有**已安装的包
- `| grep wget` — filter output to lines containing "wget" / 过滤包含"wget"的行
- Output `wget-1.14-18.el7_6.1.x86_64` shows the full package name including version and architecture / 输出显示完整包名，包括版本和架构

**Alternative — more specific query / 替代方法——更精确的查询:**
```bash
# Query specifically for wget package / 直接查询 wget 包
$ rpm -q wget
wget-1.14-18.el7_6.1.x86_64

# Show detailed info / 显示详细信息
$ rpm -qi wget

# Show all files installed by wget / 显示 wget 安装的所有文件
$ rpm -ql wget
/etc/wgetrc
/usr/bin/wget
/usr/share/doc/wget-1.14/
/usr/share/man/man1/wget.1.gz
```

---

### Exercise 3 — Install Firefox with RPM (May Fail Due to Dependencies)
### 练习 3 — 用 RPM 安装 Firefox（可能因依赖失败）

Install the Firefox browser package downloaded at `/home/bob/firefox-68.6.0-1.el7.centos.x86_64.rpm`.

安装位于 `/home/bob/firefox-68.6.0-1.el7.centos.x86_64.rpm` 的 Firefox 浏览器包。

```bash
$ sudo rpm -ivh /home/bob/firefox-68.6.0-1.el7.centos.x86_64.rpm
```

**Expected failure output / 预期失败输出:**
```
error: Failed dependencies:
    libdbus-glib-1.so.2()(64bit) is needed by firefox-68.6.0-1.el7.centos.x86_64
    libasound.so.2()(64bit) is needed by firefox-68.6.0-1.el7.centos.x86_64
    libgtk-3.so.0()(64bit) is needed by firefox-68.6.0-1.el7.centos.x86_64
    ...
```

**Why it fails / 为什么失败**: RPM does not resolve dependencies. It simply checks if the required libraries are present and fails if they're not. Each missing dependency may have its own dependencies — this cascading problem is called **"dependency hell"**.

RPM 不解析依赖关系。它只检查所需的库是否存在，如果不存在则失败。每个缺失的依赖项可能有自己的依赖项——这种级联问题被称为**"依赖地狱"**。

> **The `-ivh` flags explained / `-ivh` 标志说明:**
> - `-i` — **i**nstall the package / **安装**包
> - `-v` — **v**erbose (show detailed output) / **详细**输出
> - `-h` — show **h**ash marks as progress indicator / 显示**哈希**标记作为进度指示器

---

### Exercise 4 — Install Firefox with YUM (With Dependencies)
### 练习 4 — 用 YUM 安装 Firefox（含依赖解析）

Install Firefox along with all its dependencies using YUM.

使用 YUM 安装 Firefox 及其所有依赖项。

```bash
$ sudo yum install firefox -y
```

**What happens behind the scenes / 幕后发生了什么:**
```
1. YUM checks /etc/yum.repos.d/ for configured repositories
   YUM 检查 /etc/yum.repos.d/ 中配置的仓库

2. YUM fetches package metadata from repos
   YUM 从仓库获取包元数据

3. YUM calculates the full dependency tree:
   YUM 计算完整的依赖树：
   firefox → needs libdbus-glib → needs dbus-glib-devel → ...

4. YUM downloads ALL required packages
   YUM 下载所有需要的包

5. YUM calls RPM to install each package in the correct order
   YUM 按正确顺序调用 RPM 安装每个包

6. Installation completes successfully
   安装成功完成
```

**The `-y` flag / `-y` 标志**: Automatically answers "yes" to all prompts. Without it, YUM shows a transaction summary and asks for confirmation.

自动对所有提示回答"yes"。不加此标志时，YUM 会显示事务摘要并请求确认。

```bash
# Without -y (interactive) / 不加 -y（交互式）
$ sudo yum install firefox
...
Transaction Summary
==============================================================
Install  1 Package (+25 Dependent packages)

Total download size: 85 M
Is this ok [y/d/N]: y    ← must type 'y' manually / 需要手动输入 'y'
```

---

### Exercise 5 — Check How Many Repositories are Configured
### 练习 5 — 检查配置了多少软件仓库

```bash
$ sudo yum repolist
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
repo id                  repo name                           status
base/7/x86_64            CentOS-7 - Base                    10,072
epel/x86_64              Extra Packages for Enterprise ...   13,755
extras/7/x86_64          CentOS-7 - Extras                    498
updates/7/x86_64         CentOS-7 - Updates                  2,832
repolist: 27,157
```

**Answer / 答案**: Count the number of repo lines in the output. In this example, there are **4 repositories** configured.

统计输出中仓库行的数量。本例中配置了 **4 个仓库**。

**Understanding the output / 理解输出:**
- `repo id` — unique identifier for the repository / 仓库的唯一标识符
- `repo name` — human-readable name / 易读名称
- `status` — number of packages available in the repo / 仓库中可用包的数量

**Additional repo commands / 其他仓库命令:**
```bash
# List all repos including disabled ones / 列出所有仓库（包括已禁用的）
$ yum repolist all

# Show detailed info about a specific repo / 显示特定仓库的详细信息
$ yum repoinfo base

# View raw repo configuration files / 查看原始仓库配置文件
$ ls /etc/yum.repos.d/
$ cat /etc/yum.repos.d/CentOS-Base.repo
```

---

### Exercise 6 — Find Which Package Provides `tcpdump`
### 练习 6 — 查找哪个包提供 `tcpdump`

```bash
$ sudo yum provides tcpdump
Loaded plugins: fastestmirror
14:tcpdump-4.9.2-4.el7_7.1.x86_64 : A network traffic monitoring tool
Repo        : base
Matched from:
Filename    : /usr/sbin/tcpdump
```

**Answer / 答案**: The `tcpdump` command is provided by the `tcpdump` package (version `4.9.2-4.el7_7.1`), available in the `base` repository.

`tcpdump` 命令由 `tcpdump` 包（版本 `4.9.2-4.el7_7.1`）提供，可在 `base` 仓库中找到。

**More `yum provides` examples / 更多 `yum provides` 示例:**
```bash
# Find package providing a specific command / 查找提供特定命令的包
$ yum provides scp
$ yum provides wget
$ yum provides ifconfig

# Find package providing a specific file path / 查找提供特定文件路径的包
$ yum provides /usr/bin/python3
$ yum provides /etc/nginx/nginx.conf

# Using wildcard / 使用通配符
$ yum provides "*/bin/tcpdump"
```

> **When to use `yum provides` / 何时使用 `yum provides`**: This is invaluable when you see an error like `command not found: tcpdump` and need to know which package to install.
>
> 当你看到 `command not found: tcpdump` 之类的错误并需要知道安装哪个包时，这个命令非常有用。

---

## Summary — Lab Command Reference
## 小结 — 实验命令参考

| Task / 任务 | Command / 命令 |
|---|---|
| Find installed package name | `rpm -qa \| grep <name>` |
| Install .rpm file (no dep resolution) | `sudo rpm -ivh package.rpm` |
| Install with dep resolution | `sudo yum install <package> -y` |
| List configured repos | `sudo yum repolist` |
| Find which package provides a command | `sudo yum provides <command>` |
| Show all installed packages | `rpm -qa` |
| Query package details | `rpm -qi <package>` |
| List files in a package | `rpm -ql <package>` |
| Find package owning a file | `rpm -qf /path/to/file` |
| Search for a package | `yum search <keyword>` |
| Show package info | `yum info <package>` |
| Remove a package | `sudo yum remove <package>` |
| Update a package | `sudo yum update <package>` |
| Update all packages | `sudo yum update -y` |
