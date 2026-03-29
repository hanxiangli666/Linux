# File Compression and Archival | 文件压缩与归档

In this section, we will explore how to check file sizes, package multiple files into archives, and apply compression to reduce storage space. These skills are essential for backups, file transfers, and efficient disk usage in Linux.

本节将深入探讨如何查看文件大小、将多个文件打包成归档文件，以及通过压缩技术减少存储空间占用。这些技能在 Linux 系统的备份、文件传输和磁盘管理中至关重要。

---

## Viewing File Sizes | 查看文件大小

Before compressing files, it's helpful to know how much space they currently occupy. Linux provides several commands for this purpose.

在压缩文件之前，了解文件当前占用的空间大小非常有帮助。Linux 提供了多种命令来完成这一任务。

### `du` — Disk Usage | 磁盘使用情况

The `du` (disk usage) command reports the space used by files and directories.

`du`（disk usage，磁盘使用量）命令用于报告文件和目录所占用的磁盘空间。

**Show size in Kilobytes | 以 KB 显示文件大小：**
```bash
$ du -sk test.img
```

The `-s` flag gives a summary (total size only), and `-k` outputs the result in **kilobytes**.

`-s` 标志表示汇总输出（仅显示总大小），`-k` 表示以**千字节（KB）**为单位输出结果。

**Show size in human-readable format | 以人类可读格式显示大小：**
```bash
$ du -sh test.img
# Example output: 4.0G    test.img
# 示例输出：4.0G    test.img
```

The `-h` flag automatically chooses the most appropriate unit (KB, MB, GB), making output much easier to interpret.

`-h` 标志会自动选择最合适的单位（KB、MB、GB），使输出结果更直观易读。

**Show sizes for all files in a directory | 显示目录下所有文件的大小：**
```bash
$ du -sh /var/log/*
```

### `ls -lh` — Long Listing with Human-readable Sizes | 长格式列出并显示可读大小

The `ls` command with `-lh` is another quick way to view file sizes alongside permissions and timestamps.

`ls` 命令加上 `-lh` 参数是另一种快速查看文件大小的方式，同时还能显示权限和时间戳等信息。

```bash
$ ls -lh test.img
# Example output:
# -rw-r--r-- 1 bob bob 4.0G Jul  3 11:00 test.img
```

> **Tip | 技巧：** Use `du -sh * | sort -h` to list all items in the current directory sorted by size from smallest to largest.
>
> 使用 `du -sh * | sort -h` 可以按文件大小从小到大列出当前目录下的所有条目，非常方便做空间排查。

---

## Archiving Files | 归档文件

### What is `tar`? | 什么是 `tar`？

The `tar` command (short for **tape archive**) is the standard tool for grouping multiple files and directories into a single archive file. Unlike compression tools, `tar` itself does not compress — it only bundles. However, it can invoke compression utilities (gzip, bzip2, xz) to do both at once.

`tar` 命令（**tape archive** 的缩写）是将多个文件和目录打包成单个归档文件的标准工具。与压缩工具不同，`tar` 本身并不压缩——它只负责打包。但它可以调用压缩工具（gzip、bzip2、xz）同时完成打包和压缩。

Archive files created with `tar` are commonly called **tarballs** and typically end in `.tar`.

用 `tar` 创建的归档文件通常被称为 **tarball**，扩展名一般为 `.tar`。

### Creating an Archive | 创建归档

```bash
$ tar -cf archive.tar file1 file2 file3
```

| Option | Meaning | 选项说明 |
|--------|---------|---------|
| `-c`   | Create a new archive | 创建新归档 |
| `-f`   | Specify the archive filename | 指定归档文件名 |

**Archive an entire directory | 归档整个目录：**
```bash
$ tar -cf /home/bob/project.tar /home/bob/project/
```

**Verify the archive was created | 验证归档已创建：**
```bash
$ ls -lh archive.tar
```

### Viewing Archive Contents | 查看归档内容

The `-t` option lists the contents of a tarball without extracting it — useful for previewing before extraction.

`-t` 选项可以在不解压的情况下列出归档内容——在解压前预览非常有用。

```bash
$ tar -tf archive.tar
```

**Verbose listing (show file details) | 详细列出（显示文件详情）：**
```bash
$ tar -tvf archive.tar
```

### Extracting an Archive | 解压归档

The `-x` option extracts the contents from a tarball.

`-x` 选项用于从归档文件中提取内容。

```bash
$ tar -xf archive.tar
```

**Extract to a specific directory | 解压到指定目录：**
```bash
$ tar -xf archive.tar -C /tmp/restore/
```

### Creating a Compressed Archive (tar + gzip) | 创建压缩归档（tar + gzip）

The `-z` flag tells `tar` to pipe through `gzip` compression, creating a `.tar.gz` file in one step.

`-z` 标志告诉 `tar` 通过 `gzip` 压缩管道，一步生成 `.tar.gz` 文件。

```bash
# Create compressed archive | 创建压缩归档
$ tar -zcf archive.tar.gz /home/bob/project/

# Extract compressed archive | 解压压缩归档
$ tar -zxf archive.tar.gz

# Extract to specific directory | 解压到指定目录
$ tar -zxf archive.tar.gz -C /tmp/restore/
```

### Quick Reference: `tar` Options | 快速参考：`tar` 选项

| Option | Action | 说明 |
|--------|--------|------|
| `-c`   | Create archive | 创建归档 |
| `-x`   | Extract archive | 解压归档 |
| `-t`   | List contents | 列出内容 |
| `-f`   | Specify filename | 指定文件名 |
| `-v`   | Verbose output | 详细输出 |
| `-z`   | Use gzip compression | 使用 gzip 压缩 |
| `-j`   | Use bzip2 compression | 使用 bzip2 压缩 |
| `-J`   | Use xz compression | 使用 xz 压缩 |
| `-C`   | Change to directory | 切换到指定目录 |

---

## Compression | 压缩

Compression reduces the size of a file by encoding its data more efficiently. The reduction ratio depends on the type of data and the algorithm used. Text files compress very well; binary files may compress less.

压缩通过更高效地编码数据来减小文件大小。压缩比取决于数据类型和所用算法。文本文件压缩效果极佳，二进制文件的压缩效果相对较差。

Linux provides three widely-used compression tools:

Linux 提供了三种广泛使用的压缩工具：

### The Three Compression Tools | 三大压缩工具

| Tool | Extension | Speed | Compression Ratio | 速度 | 压缩比 |
|------|-----------|-------|-------------------|------|--------|
| `gzip` | `.gz` | Fast | Medium | 快 | 中等 |
| `bzip2` | `.bz2` | Medium | High | 中等 | 高 |
| `xz` | `.xz` | Slow | Highest | 慢 | 最高 |

> **Rule of thumb | 经验法则：** Use `gzip` for speed, `bzip2` for a balance, and `xz` when maximum compression is needed (e.g., distributing large packages).
>
> 追求速度用 `gzip`，追求平衡用 `bzip2`，需要最大压缩比（如分发大型软件包）时用 `xz`。

### Compressing Files | 压缩文件

```bash
$ bzip2 test.img        # Creates test.img.bz2 | 生成 test.img.bz2
$ gzip  test1.img       # Creates test1.img.gz | 生成 test1.img.gz
$ xz    test2.img       # Creates test2.img.xz | 生成 test2.img.xz
```

> **Note | 注意：** By default, these commands **replace** the original file with the compressed version. The original is deleted.
>
> 默认情况下，这些命令会用压缩版本**替换**原始文件，原始文件将被删除。

**Keep the original file | 保留原始文件：**
```bash
$ gzip -k test.img          # Keeps test.img AND creates test.img.gz
                             # 保留 test.img 并创建 test.img.gz
```

**Set compression level | 设置压缩级别（1=最快，9=最强）：**
```bash
$ gzip -9 test.img          # Maximum compression | 最大压缩
$ gzip -1 test.img          # Fastest compression | 最快压缩
```

### Decompressing Files | 解压文件

Each compression tool has a corresponding decompression command:

每个压缩工具都有对应的解压命令：

```bash
$ bunzip2 test.img.bz2      # Decompress bzip2 | 解压 bzip2 文件
$ gunzip  test1.img.gz      # Decompress gzip  | 解压 gzip 文件
$ unxz    test2.img.xz      # Decompress xz    | 解压 xz 文件
```

Alternatively, use the original tool with `-d` (decompress flag):

或者使用原始工具加 `-d` 标志（解压模式）：

```bash
$ gzip  -d test.img.gz
$ bzip2 -d test.img.bz2
$ xz    -d test.img.xz
```

### Reading Compressed Files Without Extracting | 不解压直接读取压缩文件

You don't always need to fully decompress a file just to read it. These tools allow you to view compressed file contents directly in the terminal:

读取压缩文件并不总是需要完整解压。以下工具可以直接在终端中查看压缩文件的内容：

```bash
$ zcat  hostfile.txt.gz     # Read gzip-compressed file  | 读取 gzip 压缩文件
$ bzcat hostfile.txt.bz2    # Read bzip2-compressed file | 读取 bzip2 压缩文件
$ xzcat hostfile.txt.xz     # Read xz-compressed file   | 读取 xz 压缩文件
```

These commands are especially useful for reading compressed log files without needing extra disk space for decompression.

这些命令对于读取压缩日志文件特别有用，无需额外磁盘空间即可完成解压。

**Combine with `grep` to search inside compressed files | 结合 `grep` 搜索压缩文件内容：**
```bash
$ zcat access.log.gz | grep "ERROR"
$ bzcat syslog.bz2   | grep "failed"
```

---

## Real-World Workflow Example | 实际工作流程示例

Here is a typical workflow for packaging a project, compressing it, transferring it, and restoring it on another machine:

以下是一个典型工作流程：打包项目、压缩、传输，然后在另一台机器上恢复。

```bash
# 1. Package and compress the project directory
#    打包并压缩项目目录
$ tar -zcf project-backup.tar.gz /home/bob/mercury/

# 2. Check the compressed file size
#    检查压缩文件大小
$ du -sh project-backup.tar.gz

# 3. Transfer to another server (using scp)
#    传输到另一台服务器（使用 scp）
$ scp project-backup.tar.gz user@server:/backup/

# 4. On the remote server, extract the archive
#    在远程服务器上解压归档
$ tar -zxf project-backup.tar.gz -C /opt/
```
