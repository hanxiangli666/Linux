# Searching for Files and Patterns | 搜索文件与文本模式

In this section, we will explore how to efficiently locate files and directories in the filesystem, and how to search for specific text patterns within files. Mastering these tools will save you enormous amounts of time during daily Linux administration.

本节将深入探讨如何在文件系统中高效定位文件和目录，以及如何在文件内容中搜索特定的文本模式。熟练掌握这些工具，将在日常 Linux 管理工作中为你节省大量时间。

The three primary tools covered in this section are:

本节涵盖三种核心搜索工具：

| Tool | Best For | 最适合的场景 |
|------|----------|-------------|
| `locate` | Quick filename lookups (uses database) | 快速文件名查找（基于数据库） |
| `find` | Precise, real-time file searches with filters | 精确的实时文件搜索（支持多种过滤条件） |
| `grep` | Searching text patterns inside file contents | 在文件内容中搜索文本模式 |

---

## `locate` — Fast File Lookup | 快速文件查找

The `locate` command is the fastest way to find files by name. It searches a pre-built database (`mlocate.db`) instead of scanning the filesystem in real time, making it extremely quick.

`locate` 命令是按文件名查找文件最快的方式。它搜索预先构建的数据库（`mlocate.db`），而不是实时扫描文件系统，因此速度极快。

```bash
$ locate City.txt
# Returns all paths containing "City.txt"
# 返回所有包含 "City.txt" 的路径
```

**Case-insensitive search | 不区分大小写搜索：**
```bash
$ locate -i city.txt
```

**Limit the number of results | 限制返回结果数量：**
```bash
$ locate -n 5 City.txt
```

### The Database Behind `locate` | `locate` 背后的数据库

The downside of `locate` is that it depends on a database that may not always be up to date. If you've just created a file, `locate` might not find it until the database is refreshed.

`locate` 的缺点是它依赖的数据库可能不是最新的。如果你刚创建了一个文件，在数据库刷新之前，`locate` 可能找不到它。

**Manually update the database | 手动更新数据库：**
```bash
$ sudo updatedb
# This must be run as root | 必须以 root 权限运行
```

The database is typically auto-updated daily by a `cron` job. You only need to run `updatedb` manually when searching for very recently created files.

数据库通常由 `cron` 定时任务每天自动更新。只有在搜索非常近期创建的文件时，才需要手动运行 `updatedb`。

> **When to use `locate` vs `find` | 何时用 `locate`，何时用 `find`：**
> - Use `locate` for **speed** when you need a quick filename lookup and the file is not brand new.
> - Use `find` for **precision** when you need real-time results, or want to filter by size, type, permissions, date, etc.
>
> - 需要**快速**查找文件名且文件不是刚创建的，用 `locate`。
> - 需要**实时准确**结果，或需要按大小、类型、权限、日期等过滤，用 `find`。

---

## `find` — Powerful Real-Time Search | 强大的实时搜索

The `find` command searches the filesystem in real time, making it more flexible and accurate than `locate`. It supports a rich set of filters to narrow down results.

`find` 命令实时扫描文件系统，比 `locate` 更灵活、更准确。它支持丰富的过滤条件，可以精准锁定目标文件。

### Basic Syntax | 基本语法

```bash
$ find [path] [options] [expression]
```

### Search by Name | 按文件名搜索

```bash
$ find /home/michael -name City.txt
```

**Case-insensitive name search | 不区分大小写的文件名搜索：**
```bash
$ find /home -iname city.txt
```

**Using wildcards | 使用通配符：**
```bash
$ find /var/log -name "*.log"          # All .log files | 所有 .log 文件
$ find /home -name "report_*.pdf"      # Pattern matching | 模式匹配
```

### Search by Type | 按类型搜索

```bash
$ find /home -type f        # Files only      | 仅文件
$ find /home -type d        # Directories only | 仅目录
$ find /home -type l        # Symbolic links  | 仅符号链接
```

### Search by Size | 按大小搜索

```bash
$ find / -size +100M        # Files larger than 100MB  | 大于 100MB 的文件
$ find / -size -1k          # Files smaller than 1KB   | 小于 1KB 的文件
$ find / -size  50M         # Files exactly 50MB       | 恰好 50MB 的文件
```

### Search by Time | 按时间搜索

```bash
$ find /home -mtime -7      # Modified in last 7 days  | 最近 7 天修改过的文件
$ find /home -mtime +30     # Not modified in 30 days  | 30 天内未修改的文件
$ find /home -newer file.txt # Newer than file.txt     | 比 file.txt 更新的文件
```

### Search by Permissions | 按权限搜索

```bash
$ find / -perm 777          # World-writable files (security risk!)
                             # 所有人可写的文件（安全风险！）
$ find / -perm -4000        # Files with SUID bit set
                             # 设置了 SUID 位的文件
```

### Execute Commands on Found Files | 对搜索结果执行命令

The `-exec` option is one of `find`'s most powerful features — it runs a command on every matched file.

`-exec` 选项是 `find` 最强大的功能之一——它对每个匹配的文件执行指定命令。

```bash
# Delete all .tmp files found under /tmp
# 删除 /tmp 下所有 .tmp 临时文件
$ find /tmp -name "*.tmp" -exec rm {} \;

# Change ownership of all files in a directory
# 批量修改目录下所有文件的所有者
$ find /home/bob -type f -exec chown bob:bob {} \;

# List details of files larger than 100MB
# 列出所有大于 100MB 的文件详情
$ find / -size +100M -exec ls -lh {} \;
```

> **Note | 注意：** `{}` is a placeholder for the matched file, and `\;` marks the end of the `-exec` command.
>
> `{}` 是匹配文件的占位符，`\;` 标记 `-exec` 命令的结束。

---

## `grep` — Search Text Patterns | 搜索文本模式

`grep` (Global Regular Expression Print) is the go-to command for searching text inside files. It is one of the most powerful and frequently used tools in Linux.

`grep`（全局正则表达式打印）是在文件内部搜索文本的首选命令，是 Linux 中最强大、使用最频繁的工具之一。

### Basic Search | 基本搜索

```bash
$ grep second sample.txt
# Prints all lines in sample.txt containing "second"
# 打印 sample.txt 中所有包含 "second" 的行
```

### Common Options | 常用选项

**Case-insensitive search | 不区分大小写搜索：**
```bash
$ grep -i capital sample.txt
```

**Recursive search in a directory | 递归搜索目录：**
```bash
$ grep -r "third Line" /home/michael
# Searches all files under /home/michael
# 搜索 /home/michael 下的所有文件
```

**Invert match (show lines that do NOT match) | 反向匹配（显示不匹配的行）：**
```bash
$ grep -v "printed" sample.txt
```

**Show line numbers | 显示行号：**
```bash
$ grep -n "error" /var/log/syslog
```

**Count matches | 统计匹配行数：**
```bash
$ grep -c "failed" /var/log/auth.log
```

### Whole Word Matching | 全词匹配

Without `-w`, grep matches substrings. For example, searching for `exam` would also match `example`, `examination`, etc.

不加 `-w` 时，grep 会匹配子字符串。例如，搜索 `exam` 也会匹配 `example`、`examination` 等。

```bash
$ grep -w exam examples.txt
# Only matches the standalone word "exam"
# 只匹配独立单词 "exam"
```

**Combine `-v` and `-w` to invert whole-word match | 组合 `-v` 和 `-w` 进行反向全词匹配：**
```bash
$ grep -vw exam examples.txt
# Shows all lines that do NOT contain the whole word "exam"
# 显示所有不包含完整单词 "exam" 的行
```

### Context Lines | 显示上下文行

Sometimes it's useful to see the lines surrounding a match for better context.

有时查看匹配行的上下文会更有帮助。

```bash
# Show 1 line AFTER each match (After)
# 每个匹配后显示 1 行
$ grep -A1 Arsenal premier-league-table.txt

# Show 1 line BEFORE each match (Before)
# 每个匹配前显示 1 行
$ grep -B1 "4th" premier-league-table.txt

# Show 1 line BEFORE and AFTER each match (Context)
# 每个匹配前后各显示 1 行
$ grep -A1 -B1 Chelsea premier-league-table.txt
```

### Searching Multiple Files | 搜索多个文件

```bash
# Search all .log files in /var/log
# 搜索 /var/log 下的所有 .log 文件
$ grep -r "Out of memory" /var/log/*.log

# Show only filenames (not matching lines)
# 只显示文件名（不显示匹配行内容）
$ grep -rl "TODO" /home/bob/project/
```

### Using `grep` with Regular Expressions | 结合正则表达式使用 `grep`

`grep` supports regular expressions, making it extremely powerful for pattern matching.

`grep` 支持正则表达式，这使其在模式匹配方面极为强大。

```bash
# Lines starting with "Error"
# 以 "Error" 开头的行
$ grep "^Error" /var/log/app.log

# Lines ending with a number
# 以数字结尾的行
$ grep "[0-9]$" data.txt

# Extended regex: match "color" or "colour"
# 扩展正则：匹配 "color" 或 "colour"
$ grep -E "colou?r" document.txt

# Match IP address pattern
# 匹配 IP 地址模式
$ grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" /etc/hosts
```

---

## Combining Tools | 工具组合使用

The real power comes from combining these tools together with pipes:

真正的威力在于通过管道将这些工具组合在一起使用：

```bash
# Find all Python files and search for "import" statements
# 查找所有 Python 文件并搜索 "import" 语句
$ find /home/bob -name "*.py" | xargs grep "import os"

# Find large log files and check if they contain errors
# 查找大型日志文件并检查是否包含错误
$ find /var/log -size +10M -name "*.log" -exec grep -l "ERROR" {} \;

# Count how many .conf files contain the word "port"
# 统计有多少 .conf 文件包含单词 "port"
$ find /etc -name "*.conf" | xargs grep -l "port" | wc -l
```
