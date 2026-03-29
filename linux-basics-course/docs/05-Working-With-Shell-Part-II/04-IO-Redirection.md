# IO Redirection | 输入/输出重定向

In this section, we will take a deep look at IO Redirection — one of the most fundamental and powerful concepts in Linux shell scripting and command-line usage. Understanding how data flows between commands, files, and the terminal will make you a significantly more effective Linux user.

本节将深入讲解 IO 重定向——这是 Linux shell 脚本编写和命令行使用中最基础、最强大的概念之一。理解数据在命令、文件和终端之间的流转方式，将极大地提升你的 Linux 使用效率。

---

## Standard Streams | 标准数据流

Every command launched in Linux automatically has three communication channels, known as **standard streams**. Think of them as pipes through which data flows in and out of a program.

Linux 中启动的每个命令都自动拥有三个通信通道，称为**标准数据流**。可以把它们想象成数据流入和流出程序的管道。

| Stream | File Descriptor | Default Destination | 说明 |
|--------|----------------|---------------------|------|
| **STDIN** (Standard Input)  | `0` | Keyboard | 标准输入，默认来自键盘 |
| **STDOUT** (Standard Output) | `1` | Terminal screen | 标准输出，默认输出到终端屏幕 |
| **STDERR** (Standard Error)  | `2` | Terminal screen | 标准错误，默认输出到终端屏幕 |

```
┌──────────────┐
│   Keyboard   │ ──── STDIN (0) ────▶ ┌─────────────┐ ──── STDOUT (1) ────▶ Screen
└──────────────┘                      │   Command   │
                                      └─────────────┘ ──── STDERR (2) ────▶ Screen
```

With **IO Redirection**, you can change where these streams read from or write to — typically redirecting them to files, or connecting them to other commands via **pipes**.

通过 **IO 重定向**，你可以改变这些数据流的读取源或写入目标——通常是重定向到文件，或通过**管道**将其连接到其他命令。

---

## Redirecting STDOUT | 重定向标准输出

### Overwrite with `>` | 使用 `>` 覆盖写入

The `>` operator redirects STDOUT to a file. If the file already exists, it is **overwritten**. If it doesn't exist, it is created.

`>` 运算符将 STDOUT 重定向到文件。如果文件已存在，内容将被**覆盖**；如果不存在，则自动创建。

```bash
$ echo $SHELL > shell.txt
# Writes the shell path to shell.txt (creates or overwrites)
# 将 shell 路径写入 shell.txt（创建或覆盖）

$ ls -l /home > filelist.txt
# Save directory listing to a file
# 将目录列表保存到文件
```

### Append with `>>` | 使用 `>>` 追加写入

The `>>` operator appends STDOUT to an existing file without overwriting its content.

`>>` 运算符将 STDOUT 追加到已有文件末尾，不会覆盖原有内容。

```bash
$ echo $SHELL >> shell.txt
# Appends the shell path to shell.txt
# 将 shell 路径追加到 shell.txt

$ date >> activity.log
# Add a timestamp to a running log
# 向运行日志中追加时间戳
```

> **Key difference | 关键区别：**
> - `>`  replaces the file content each time. | 每次都会替换文件内容。
> - `>>` adds to the end of the file. | 向文件末尾追加内容。

---

## Redirecting STDERR | 重定向标准错误

Error messages are sent to STDERR (file descriptor `2`), not STDOUT. They are handled separately, which gives you fine-grained control over logging.

错误消息发送到 STDERR（文件描述符 `2`），而不是 STDOUT。它们被单独处理，让你对日志记录有精细的控制权。

### Redirect errors to a file | 将错误重定向到文件

```bash
$ cat missing_file 2> error.txt
# The "2>" means "redirect file descriptor 2 (STDERR) to this file"
# "2>" 意为"将文件描述符 2（STDERR）重定向到此文件"
```

### Append errors to a file | 将错误追加到文件

```bash
$ cat missing_file 2>> error.txt
```

### Discard errors silently | 静默丢弃错误

`/dev/null` is a special "black hole" device — anything written to it is discarded immediately.

`/dev/null` 是一个特殊的"黑洞"设备——写入其中的任何内容都会被立即丢弃。

```bash
$ cat missing_file 2> /dev/null
# Error is silently discarded, nothing printed to screen
# 错误被静默丢弃，屏幕上不显示任何内容
```

This is commonly used in scripts to suppress unwanted error messages.

这在脚本中常用于抑制不需要的错误消息。

### Redirect BOTH STDOUT and STDERR | 同时重定向 STDOUT 和 STDERR

```bash
# Redirect stdout to output.txt, stderr to error.txt
# 将 stdout 重定向到 output.txt，stderr 重定向到 error.txt
$ command > output.txt 2> error.txt

# Redirect both stdout and stderr to the same file
# 将 stdout 和 stderr 都重定向到同一文件
$ command > all_output.txt 2>&1

# Modern shorthand (bash 4+)
# 现代简写方式（bash 4+）
$ command &> all_output.txt
```

> **Understanding `2>&1` | 理解 `2>&1`：**
> This means "redirect file descriptor 2 (STDERR) to wherever file descriptor 1 (STDOUT) is currently pointing."
>
> 这意味着"将文件描述符 2（STDERR）重定向到文件描述符 1（STDOUT）当前指向的位置"。

---

## Redirecting STDIN | 重定向标准输入

Just as you can redirect output, you can also redirect input. The `<` operator tells a command to read from a file instead of the keyboard.

就像可以重定向输出一样，也可以重定向输入。`<` 运算符告诉命令从文件而不是键盘读取数据。

```bash
$ sort < unsorted_list.txt
# Reads from file instead of waiting for keyboard input
# 从文件读取而不是等待键盘输入

$ mail -s "Report" admin@example.com < report.txt
# Send file contents as email body
# 将文件内容作为邮件正文发送
```

---

## Command Line Pipes | 命令行管道

Pipes (`|`) are one of Linux's most elegant features. They connect the **STDOUT of one command directly to the STDIN of the next**, allowing you to build powerful processing chains.

管道（`|`）是 Linux 最优雅的特性之一。它将**一个命令的 STDOUT 直接连接到下一个命令的 STDIN**，让你可以构建强大的数据处理链。

```
Command1 ──STDOUT──▶ | ──STDIN──▶ Command2 ──STDOUT──▶ | ──STDIN──▶ Command3
```

### Basic Pipe Examples | 基本管道示例

```bash
# Search for "Hello" in file, then view results page by page
# 在文件中搜索 "Hello"，然后逐页查看结果
$ grep Hello sample.txt | less

# List all processes and search for a specific one
# 列出所有进程并搜索特定进程
$ ps aux | grep nginx

# Count the number of files in a directory
# 统计目录中的文件数量
$ ls /etc | wc -l

# Find the 5 largest files in the current directory
# 找出当前目录中最大的 5 个文件
$ du -sh * | sort -rh | head -5

# Show unique failed login attempts from auth log
# 显示认证日志中的唯一失败登录记录
$ cat /var/log/auth.log | grep "Failed" | awk '{print $11}' | sort | uniq -c | sort -rn
```

### Multi-stage Pipeline | 多级管道处理

```bash
# Extract, filter, and count — a classic three-stage pipeline
# 提取、过滤、统计——经典三级管道
$ cat /var/log/syslog | grep "error" | wc -l
```

---

## The `tee` Command | `tee` 命令

The `tee` command solves a common problem: you want to see the output on screen **and** save it to a file at the same time. It reads from STDIN and writes to **both** STDOUT and a file simultaneously — like a T-junction in a pipe.

`tee` 命令解决了一个常见问题：你希望同时在屏幕上查看输出**并**将其保存到文件中。它从 STDIN 读取，同时写入 **STDOUT 和文件**——就像管道中的 T 形接头。

```bash
$ echo $SHELL | tee shell.txt
# Prints to screen AND writes to shell.txt
# 打印到屏幕，同时写入 shell.txt
```

### Append mode with `tee -a` | 使用 `tee -a` 追加模式

```bash
$ echo "This is the bash shell" | tee -a shell.txt
# Appends to shell.txt instead of overwriting
# 追加到 shell.txt 而不是覆盖
```

### `tee` in multi-step pipelines | `tee` 在多步骤管道中的应用

`tee` is especially useful in the middle of a pipeline when you want to capture an intermediate result:

当你需要捕获中间结果时，`tee` 在管道中间位置特别有用：

```bash
$ cat /var/log/syslog | grep "ERROR" | tee errors_found.txt | wc -l
# Saves matching errors to file AND counts them
# 将匹配的错误保存到文件，同时统计数量
```

---

## Summary: Redirection Quick Reference | 汇总：重定向快速参考

| Symbol | Action | 说明 |
|--------|--------|------|
| `>`    | Redirect STDOUT to file (overwrite) | 将 STDOUT 重定向到文件（覆盖） |
| `>>`   | Redirect STDOUT to file (append)    | 将 STDOUT 重定向到文件（追加） |
| `<`    | Redirect file to STDIN              | 将文件重定向到 STDIN |
| `2>`   | Redirect STDERR to file (overwrite) | 将 STDERR 重定向到文件（覆盖） |
| `2>>`  | Redirect STDERR to file (append)    | 将 STDERR 重定向到文件（追加） |
| `2>&1` | Redirect STDERR to same dest as STDOUT | 将 STDERR 重定向到与 STDOUT 相同的目标 |
| `&>`   | Redirect both STDOUT and STDERR to file | 将 STDOUT 和 STDERR 都重定向到文件 |
| `\|`   | Pipe STDOUT of one command to STDIN of next | 将一个命令的 STDOUT 通过管道传给下一个命令的 STDIN |
| `tee`  | Write to both STDOUT and a file     | 同时写入 STDOUT 和文件 |
| `/dev/null` | Discard output silently        | 静默丢弃输出 |
