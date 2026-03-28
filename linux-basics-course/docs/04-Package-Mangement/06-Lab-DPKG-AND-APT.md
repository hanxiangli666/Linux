# Lab - DPKG and APT
# 实验 - DPKG 与 APT

- Access Hands-On Labs here [Hands-On Labs](https://kodekloud.com/topic/lab-dpkg-and-apt-2/)

This lab covers practical exercises using DPKG and APT on a Debian/Ubuntu system. Each exercise includes the command, detailed explanation, and additional tips.

本实验涵盖在 Debian/Ubuntu 系统上使用 DPKG 和 APT 的实践练习。每道题都包含命令、详细解释以及额外技巧。

---

### Exercise 1 — Which Package Managers to Use on Debian
### 练习 1 — Debian 上使用哪些包管理器

**Question / 问题**: Which package managers would you use on a Debian-based distribution?

**Answer / 答案**:

| Tool / 工具 | Type / 类型 | Use Case / 使用场景 |
|---|---|---|
| `dpkg` | Low-level / 底层 | Install/inspect individual `.deb` files; query installed packages / 安装/检查单个 `.deb` 文件；查询已安装的包 |
| `apt` | High-level / 高层 | Day-to-day package management with automatic dependency resolution / 日常包管理，自动解析依赖 |
| `apt-get` | High-level / 高层 | Same as `apt` but better for scripts / 与 `apt` 相同，但更适合脚本 |

```
User request: "Install nginx"
用户请求："安装 nginx"
         │
         ▼
    apt install nginx
         │
         ▼
  Fetches metadata, resolves dependencies
  获取元数据，解析依赖
         │
         ▼
    dpkg -i nginx.deb  ← apt calls dpkg internally / apt 内部调用 dpkg
```

---

### Exercise 2 — Install Firefox with DPKG (May Fail)
### 练习 2 — 用 DPKG 安装 Firefox（可能失败）

Install the Firefox browser package downloaded at `/root/firefox.deb`. Note: the dependencies might fail.

安装位于 `/root/firefox.deb` 的 Firefox 浏览器包。注意：依赖项可能会失败。

```bash
$ sudo dpkg -i /root/firefox.deb
```

**Expected output with dependency errors / 预期的依赖错误输出:**
```
(Reading database ... 85000 files and directories currently installed.)
Preparing to unpack /root/firefox.deb ...
Unpacking firefox (75.0+build3-0ubuntu1) over (75.0+build2) ...
dpkg: dependency problems prevent configuration of firefox:
 firefox depends on libdbus-glib-1-2 (>= 0.78); however:
  Package libdbus-glib-1-2 is not installed.
 firefox depends on libgtk-3-0 (>= 3.14); however:
  Package libgtk-3-0 is not installed.

dpkg: error processing package firefox (--install):
 dependency problems - leaving unconfigured
Errors were encountered while processing:
 firefox
```

**Fix broken dependencies after dpkg failure / 修复 dpkg 失败后的损坏依赖:**
```bash
# This fixes packages left in a broken state by dpkg
# 修复 dpkg 留下的损坏状态的包
$ sudo apt-get install -f
# or / 或者
$ sudo apt --fix-broken install
```

> **Why does dpkg fail but apt succeeds? / 为什么 dpkg 失败而 apt 成功?**: `dpkg` is like a screwdriver — it can install exactly what you give it, but it won't automatically go get the other parts you need. `apt` is like a contractor who reads the blueprint, goes to the hardware store, buys all the necessary materials, and assembles everything correctly.
>
> `dpkg` 就像一把螺丝刀——它可以精确安装你给它的东西，但不会自动获取你需要的其他部件。`apt` 就像一个承包商，它阅读蓝图，去五金店，购买所有必要的材料，并正确组装一切。

---

### Exercise 3 — Install Firefox with APT (With Dependencies)
### 练习 3 — 用 APT 安装 Firefox（含依赖解析）

Install Firefox using APT, which automatically handles all dependencies.

使用 APT 安装 Firefox，它会自动处理所有依赖项。

```bash
$ sudo apt install firefox
```

**Full workflow / 完整工作流程:**
```bash
# Step 1: Update package index (recommended before installing)
# 步骤 1：更新包索引（安装前推荐）
$ sudo apt update

# Step 2: Install Firefox with all dependencies
# 步骤 2：安装 Firefox 及所有依赖项
$ sudo apt install firefox

# Step 3: Verify installation / 步骤 3：验证安装
$ firefox --version
Mozilla Firefox 75.0
```

**Install without confirmation / 安装时不需要确认:**
```bash
$ sudo apt install -y firefox
```

**What APT does that DPKG doesn't / APT 做到而 DPKG 没做到的:**
```
apt install firefox
    ├── Reads /etc/apt/sources.list / 读取 sources.list
    ├── Downloads package metadata / 下载包元数据
    ├── Calculates dependency tree / 计算依赖树
    │   firefox
    │   ├── needs libdbus-glib-1-2
    │   ├── needs libgtk-3-0
    │   └── needs libglib2.0-0
    ├── Downloads ALL required .deb files / 下载所有需要的 .deb 文件
    └── Calls dpkg to install each one in order / 按顺序调用 dpkg 安装
```

---

### Exercise 4 — Search for the Chromium Browser Package
### 练习 4 — 搜索 Chromium 浏览器包

Use `apt search` to locate the correct package name for the Chromium browser. The package has the description: *"Chromium web browser, open-source version of Chrome"*.

使用 `apt search` 查找 Chromium 浏览器的正确包名。该包的描述为：*"Chromium web browser, open-source version of Chrome"*。

```bash
$ apt search chromium-browser
Sorting... Done
Full Text Search... Done

chromium-browser/focal 85.0.4183.121-0ubuntu0.20.04.1 amd64
  Chromium web browser, open-source version of Chrome
```

**Answer / 答案**: The package name is **`chromium-browser`**.

**More search techniques / 更多搜索技巧:**
```bash
# Search by keyword / 按关键词搜索
$ apt search chromium

# Search only in package names / 只在包名中搜索
$ apt search --names-only chromium

# Show detailed info about the package / 显示包的详细信息
$ apt show chromium-browser
Package: chromium-browser
Version: 85.0.4183.121-0ubuntu0.20.04.1
Priority: optional
Section: web
Maintainer: Ubuntu Chromium Team
Installed-Size: 227 MB
Depends: chromium-browser-l10n (= 85.0...), libc6 (>= 2.17), ...
Description: Chromium web browser, open-source version of Chrome
 An open-source browser project that aims to build a safer, faster,
 and more stable way for all Internet users to experience the web.
```

> **`apt search` vs `apt-cache search` / 区别**: Both search the local package index. `apt search` performs a **full text search** (name + description), while `apt-cache search` uses regex matching. Both require `apt update` to have been run recently for accurate results.
>
> 两者都搜索本地包索引。`apt search` 执行**全文搜索**（名称+描述），而 `apt-cache search` 使用正则表达式匹配。两者都需要最近运行过 `apt update` 才能获得准确结果。

---

### Exercise 5 — Install Chromium Browser
### 练习 5 — 安装 Chromium 浏览器

```bash
$ sudo apt install -y chromium-browser
```

**Explanation / 说明:**
- `-y` flag automatically answers "yes" to all prompts / `-y` 标志自动对所有提示回答"yes"
- APT will calculate and install all required dependencies automatically / APT 将自动计算并安装所有必需的依赖项
- Installation progress will be shown with a progress bar / 安装进度将以进度条显示

**Verify the installation / 验证安装:**
```bash
$ chromium-browser --version
Chromium 85.0.4183.121 Built on Ubuntu , running on Ubuntu 20.04

# Check where it was installed / 检查安装位置
$ which chromium-browser
/usr/bin/chromium-browser

# Check package status / 检查包状态
$ dpkg -s chromium-browser | grep Status
Status: install ok installed
```

---

### Exercise 6 — Remove Firefox
### 练习 6 — 删除 Firefox

Remove the Firefox browser from the system.

从系统中删除 Firefox 浏览器。

```bash
$ sudo apt remove firefox
```

**Understand the difference between remove and purge / 理解 remove 和 purge 的区别:**

```bash
# Remove: uninstall the package, KEEP configuration files
# 删除：卸载包，保留配置文件
$ sudo apt remove firefox

# Purge: uninstall the package AND DELETE configuration files
# 清除：卸载包并删除配置文件
$ sudo apt purge firefox

# Autoremove: remove packages that were auto-installed as deps
# and are no longer needed
# 自动删除：删除作为依赖自动安装且不再需要的包
$ sudo apt autoremove

# Complete removal (remove + purge + autoremove) / 完全删除
$ sudo apt purge firefox && sudo apt autoremove
```

**Verify removal / 验证删除:**
```bash
$ dpkg -s firefox
dpkg-query: package 'firefox' is not installed and no information is available

$ which firefox
# (no output = not found) / （无输出 = 未找到）
```

> **When to use `purge` vs `remove` / 何时使用 `purge` vs `remove`**:
> - Use `remove` when you plan to **reinstall later** and want to keep your settings / 当你计划**之后重新安装**并希望保留设置时使用 `remove`
> - Use `purge` when you want a **completely clean uninstall** with no config file traces / 当你想要**完全干净地卸载**且不留配置文件痕迹时使用 `purge`

---

## Bonus — Essential APT Workflow
## 附加题 — 基本 APT 工作流程

```bash
# Complete system update workflow / 完整系统更新工作流程
$ sudo apt update          # 1. Refresh package lists / 刷新包列表
$ apt list --upgradable    # 2. See what can be upgraded / 查看可升级的包
$ sudo apt upgrade -y      # 3. Upgrade all packages / 升级所有包
$ sudo apt autoremove -y   # 4. Remove unneeded packages / 删除不需要的包
$ sudo apt clean           # 5. Clean download cache / 清理下载缓存
```

```bash
# Investigate a package before installing / 安装前调查包
$ apt search nginx          # Find the package / 查找包
$ apt show nginx            # Read its description and deps / 阅读描述和依赖
$ sudo apt install nginx    # Install it / 安装

# Find and fix issues / 查找并修复问题
$ sudo apt-get install -f   # Fix broken dependencies / 修复损坏的依赖
$ sudo dpkg --configure -a  # Configure unpacked packages / 配置已解包的包
$ sudo apt clean && sudo apt update  # Reset and refresh / 重置并刷新
```

---

## Summary — Lab Command Reference
## 小结 — 实验命令参考

| Task / 任务 | Command / 命令 |
|---|---|
| Install .deb without dep resolution / 安装 .deb（不解析依赖）| `sudo dpkg -i /path/file.deb` |
| Fix broken deps after dpkg failure / 修复 dpkg 失败后的依赖 | `sudo apt --fix-broken install` |
| Install with dep resolution / 安装（含依赖解析）| `sudo apt install <pkg>` |
| Install without confirmation / 安装时不确认 | `sudo apt install -y <pkg>` |
| Search for a package / 搜索包 | `apt search <keyword>` |
| Show package details / 显示包详情 | `apt show <pkg>` |
| Remove (keep config) / 删除（保留配置）| `sudo apt remove <pkg>` |
| Remove (delete config) / 删除（删除配置）| `sudo apt purge <pkg>` |
| Remove unused dependencies / 删除未使用的依赖 | `sudo apt autoremove` |
| Update package index / 更新包索引 | `sudo apt update` |
| Upgrade all packages / 升级所有包 | `sudo apt upgrade -y` |
| List installed packages / 列出已安装的包 | `dpkg -l` or `apt list --installed` |
| Check if package is installed / 检查包是否已安装 | `dpkg -s <pkg>` |
| Find package owning a file / 查找文件归属的包 | `dpkg -S /path/to/file` |
