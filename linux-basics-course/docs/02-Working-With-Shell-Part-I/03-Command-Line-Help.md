# Using the Command Line to Get Help
# 使用命令行获取帮助

- Take me to the [Video Tutorial](https://kodekloud.com/topic/command-line-help-3/)

If you are new to Linux or the Bash shell, or if you are unsure what a particular command does, there are several built-in tools that can help you quickly find the information you need — without leaving the terminal.

如果你刚开始使用 Linux 或 Bash Shell，或者不确定某个命令的用途，有几个内置工具可以帮你在不离开终端的情况下快速找到所需信息。

Linux's built-in help system is one of its greatest strengths. Learning to use it effectively means you can work productively even without an internet connection.

Linux 内置的帮助系统是其最大优势之一。熟练使用它意味着即使没有网络连接，你也能高效工作。

---

## `whatis` — One-line Command Description
## `whatis` — 单行命令描述

The `whatis` command displays a **brief, one-line description** of what a command does. It's perfect for a quick reminder when you know the command name but forget its purpose.

`whatis` 命令显示某个命令用途的**简短单行描述**。当你记得命令名但忘了它的作用时，这是一个快速提醒的好工具。

**Syntax / 语法:**
```bash
whatis <command>
```

**Examples / 示例:**
```bash
$ whatis date
date (1)             - print or set the system date and time

$ whatis ls
ls (1)               - list directory contents

$ whatis passwd
passwd (1)           - change user password
passwd (5)           - password file

$ whatis cp
cp (1)               - copy files and directories
```

> **Note / 注意**: The number in parentheses (e.g., `(1)`, `(5)`) refers to the **manual section**:
> - Section 1: User commands / 用户命令
> - Section 5: File formats and configuration files / 文件格式与配置文件
> - Section 8: System administration commands / 系统管理命令
>
> 括号中的数字（如 `(1)`、`(5)`）表示**手册章节**：
> - 第 1 章：用户命令
> - 第 5 章：文件格式与配置文件
> - 第 8 章：系统管理命令

> **Troubleshooting / 故障排除**: If `whatis` returns "nothing appropriate", the manual database may need updating. Run `sudo mandb` to rebuild it.
>
> 如果 `whatis` 返回"nothing appropriate"，手册数据库可能需要更新。运行 `sudo mandb` 重建数据库。

---

## `man` — Manual Pages
## `man` — 手册页

The `man` command provides **comprehensive documentation** for commands, including a full description, all available options, usage examples, related commands, and configuration details.

`man` 命令提供命令的**完整文档**，包括详细描述、所有可用选项、使用示例、相关命令和配置细节。

**Syntax / 语法:**
```bash
man <command>
```

**Examples / 示例:**
```bash
$ man date
$ man ls
$ man passwd
```

**Structure of a man page / 手册页的结构:**

A typical `man` page contains these sections:

典型的 `man` 页面包含以下章节：

| Section / 章节 | Content / 内容 |
|---|---|
| `NAME` | Command name and brief description / 命令名和简短描述 |
| `SYNOPSIS` | Syntax summary with all options / 包含所有选项的语法摘要 |
| `DESCRIPTION` | Detailed explanation of the command / 命令的详细说明 |
| `OPTIONS` | All flags and their effects / 所有标志及其作用 |
| `EXAMPLES` | Practical usage examples / 实际使用示例 |
| `FILES` | Related configuration or data files / 相关配置或数据文件 |
| `SEE ALSO` | Related commands / 相关命令 |
| `BUGS` | Known issues / 已知问题 |
| `AUTHOR` | Who wrote the command / 命令作者 |

**Navigation keys inside `man` / `man` 内部导航快捷键:**

| Key / 按键 | Action / 操作 |
|---|---|
| `Space` / `f` | Scroll down one page / 向下翻一页 |
| `b` | Scroll up one page / 向上翻一页 |
| `↑` / `↓` | Scroll one line / 逐行滚动 |
| `/keyword` | Search for keyword / 搜索关键词 |
| `n` | Next search match / 下一个匹配 |
| `N` | Previous search match / 上一个匹配 |
| `g` | Go to beginning / 跳到开头 |
| `G` | Go to end / 跳到末尾 |
| `q` | Quit / 退出 |

**Accessing a specific manual section / 访问特定手册章节:**
```bash
# View section 5 of passwd (the file format, not the command)
# 查看 passwd 的第 5 章（文件格式，而非命令）
$ man 5 passwd

# View section 1 of passwd (the command)
# 查看 passwd 的第 1 章（命令）
$ man 1 passwd
```

> **Pro tip / 专业技巧**: Use `man -k keyword` to search all man pages for a keyword — this is equivalent to `apropos` (described below).
>
> 使用 `man -k keyword` 可以在所有手册页中搜索关键词，这等同于下面介绍的 `apropos` 命令。

---

## `--help` / `-h` — Inline Help
## `--help` / `-h` — 内联帮助

Most commands support the `--help` (or sometimes `-h`) flag, which prints a **quick reference** of available options directly to the terminal. This is faster than `man` when you just need to recall a specific option.

大多数命令支持 `--help`（有时是 `-h`）标志，它会直接在终端打印可用选项的**快速参考**。当你只需要回忆某个具体选项时，这比 `man` 更快。

```bash
$ date --help
$ ls --help
$ cp --help
$ mkdir --help
```

**Example output / 输出示例:**
```bash
$ mkdir --help
Usage: mkdir [OPTION]... DIRECTORY...
Create the DIRECTORY(ies), if they do not already exist.

  -m, --mode=MODE   set file mode (as in chmod), not a=rwx - umask
  -p, --parents     no error if existing, make parent directories as needed
  -v, --verbose     print a message for each created directory
  -Z                set SELinux security context of each created directory
      --help        display this help and exit
      --version     output version information and exit
```

> **`--help` vs `man` / 区别**:
> - `--help` gives a quick, concise overview — best for a fast option lookup.
> - `man` gives the complete reference — best for deep understanding.
>
> - `--help` 提供简洁的快速概览，适合快速查找选项。
> - `man` 提供完整参考文档，适合深入理解。

> **Note / 注意**: Some commands use `-h` instead of `--help`, and a few use neither. When in doubt, try both or consult `man`.
>
> 某些命令使用 `-h` 而非 `--help`，少数命令两者都不支持。不确定时，两者都试一下，或查阅 `man`。

---

## `apropos` — Search Commands by Keyword
## `apropos` — 按关键词搜索命令

The `apropos` command searches through **all man page names and descriptions** for a given keyword. It's extremely useful when you know *what you want to do* but don't know *which command to use*.

`apropos` 命令在**所有手册页的名称和描述**中搜索指定关键词。当你知道*想做什么*但不知道*用哪个命令*时，它非常有用。

**Syntax / 语法:**
```bash
apropos <keyword>
```

**Examples / 示例:**
```bash
# Find all commands related to "password" / 查找所有与"password"相关的命令
$ apropos password
chage (1)            - change user password expiry information
chpasswd (8)         - update passwords in batch mode
gpasswd (1)          - administer /etc/group and /etc/gshadow
passwd (1)           - change user password
passwd (5)           - password file

# Find commands related to "network" / 查找与"network"相关的命令
$ apropos network

# Find commands related to disk partitioning / 查找与磁盘分区相关的命令
$ apropos partition

# Find commands related to a module / 查找与模块相关的命令
$ apropos modpr
```

> **`apropos` is equivalent to `man -k` / `apropos` 等同于 `man -k`**:
> ```bash
> $ apropos password
> # is the same as / 等同于
> $ man -k password
> ```

> **Troubleshooting / 故障排除**: If `apropos` returns "nothing appropriate" for everything, the man database needs to be rebuilt:
> ```bash
> $ sudo mandb
> ```
> 如果 `apropos` 对所有查询都返回"nothing appropriate"，需要重建手册数据库：
> ```bash
> $ sudo mandb
> ```

---

## `info` — Alternative to `man` (GNU Info)
## `info` — `man` 的替代方案（GNU Info）

Some GNU tools provide more detailed documentation through the `info` system, which supports hyperlinks between topics.

一些 GNU 工具通过 `info` 系统提供更详细的文档，支持主题之间的超链接。

```bash
$ info ls
$ info date
$ info coreutils
```

Navigation inside `info` / `info` 内部导航:
- `n` — next node / 下一节点
- `p` — previous node / 上一节点
- `u` — up to parent node / 返回上级节点
- `Enter` — follow a link / 跟随链接
- `q` — quit / 退出

---

## `type` and `which` — Locate Commands
## `type` 和 `which` — 定位命令

```bash
# Show what a command is and where it is / 显示命令的类型和位置
$ type ls
ls is aliased to 'ls --color=auto'

$ type echo
echo is a shell builtin

$ type date
date is /bin/date

# Find the full path of an external command / 查找外部命令的完整路径
$ which python3
/usr/bin/python3

$ which ls
/bin/ls
```

---

## Summary — Help Commands at a Glance
## 小结 — 帮助命令一览

| Command / 命令 | Use When / 使用场景 | Speed / 速度 |
|---|---|---|
| `whatis <cmd>` | Need a one-line reminder of what a command does / 需要命令用途的一行提示 | Fast / 快 |
| `man <cmd>` | Need complete, detailed documentation / 需要完整详细的文档 | Moderate / 中等 |
| `<cmd> --help` | Need a quick option reference / 需要快速查找选项 | Fast / 快 |
| `apropos <keyword>` | Don't know which command to use / 不知道用哪个命令 | Moderate / 中等 |
| `info <cmd>` | Need hyperlinked GNU documentation / 需要带超链接的 GNU 文档 | Moderate / 中等 |
| `type <cmd>` | Check if command is built-in or external / 检查命令是内置还是外部 | Fast / 快 |
| `which <cmd>` | Find the path of an external command / 查找外部命令的路径 | Fast / 快 |
